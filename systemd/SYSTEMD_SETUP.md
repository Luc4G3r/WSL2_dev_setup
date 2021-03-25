[See instruction source](https://github.com/DamionGans/ubuntu-wsl2-systemd-script)
**_NOTE: The script from the git repo above may cause issues with snap on your system, depending on your user setup. Replace the created files with following contents:_**
* `/usr/sbin/start-systemd-namespace`
```
#!/bin/sh

SYSTEMD_EXE="/lib/systemd/systemd --system-unit=multi-user.target"
SYSTEMD_PID="$(ps -eo pid=,args= | awk '$2" "$3=="'"$SYSTEMD_EXE"'" {print $1}')"
if [ "$LOGNAME" != "root" ] && ( [ -z "$SYSTEMD_PID" ] || [ "$SYSTEMD_PID" != "1" ] ); then
    export | sed -e 's/^declare -x //;/^IFS=".*[^"]$/{N;s/\n//}' | \
        grep -E -v "^(BASH|BASH_ENV|DIRSTACK|EUID|GROUPS|HOME|HOSTNAME|\
IFS|LANG|LOGNAME|MACHTYPE|MAIL|NAME|OLDPWD|OPTERR|\
OSTYPE|PATH|PIPESTATUS|POSIXLY_CORRECT|PPID|PS1|PS4|\
SHELL|SHELLOPTS|SHLVL|SYSTEMD_PID|UID|USER|_)(=|\$)" > "$HOME/.systemd-env"
    export PRE_NAMESPACE_PATH="$PATH"
    export PRE_NAMESPACE_PWD="$(pwd)"
    exec sudo /usr/sbin/enter-systemd-namespace "$BASH_EXECUTION_STRING"
fi
if [ -n "$PRE_NAMESPACE_PATH" ]; then
    export PATH="$PRE_NAMESPACE_PATH"
    unset PRE_NAMESPACE_PATH
fi
if [ -n "$PRE_NAMESPACE_PWD" ]; then
    cd "$PRE_NAMESPACE_PWD"
    unset PRE_NAMESPACE_PWD
fi
```
* `/usr/sbin/enter-systemd-namespace`
```
#!/bin/bash --norc

if [ "$LOGNAME" != "root" ]; then
    echo "You need to run $0 through sudo"
    exit 1
fi

if [ -x /usr/sbin/daemonize ]; then
    DAEMONIZE=/usr/sbin/daemonize
elif [ -x /usr/bin/daemonize ]; then
    DAEMONIZE=/usr/bin/daemonize
else
    echo "Cannot execute daemonize to start systemd."
    exit 1
fi

if ! command -v /lib/systemd/systemd > /dev/null; then
    echo "Cannot execute /lib/systemd/systemd."
    exit 1
fi

if ! command -v /usr/bin/unshare > /dev/null; then
    echo "Cannot execute /usr/bin/unshare."
    exit 1
fi

SYSTEMD_EXE="/lib/systemd/systemd --system-unit=multi-user.target"
SYSTEMD_PID="$(ps -eo pid=,args= | awk '$2" "$3=="'"$SYSTEMD_EXE"'" {print $1}')"
if [ -z "$SYSTEMD_PID" ]; then
    "$DAEMONIZE" /usr/bin/unshare --fork --pid --mount-proc bash -c 'export container=wsl; mount -t binfmt_misc binfmt_misc /proc/sys/fs/binfmt_misc; exec '"$SYSTEMD_EXE"
    while [ -z "$SYSTEMD_PID" ]; do
        echo "Sleeping for 1 second to let systemd settle"
        sleep 1
        SYSTEMD_PID="$(ps -eo pid=,args= | awk '$2" "$3=="'"$SYSTEMD_EXE"'" {print $1}')"
    done
fi

USER_HOME="$(getent passwd | awk -F: '$1=="'"$SUDO_USER"'" {print $6}')"
if [ -n "$SYSTEMD_PID" ] && [ "$SYSTEMD_PID" != "1" ]; then
    if [ -n "$1" ] && [ "$1" != "bash --login" ] && [ "$1" != "/bin/bash --login" ]; then
        exec /usr/bin/nsenter -t "$SYSTEMD_PID" -a \
            /usr/bin/sudo -H -u "$SUDO_USER" \
            /bin/bash -c 'set -a; [ -f "$HOME/.systemd-env" ] && source "$HOME/.systemd-env"; set +a; exec bash -c '"$(printf "%q" "$@")"
    else
        exec /usr/bin/nsenter -t "$SYSTEMD_PID" -a \
            /bin/login -p -f "$SUDO_USER" \
            $([ -f "$USER_HOME/.systemd-env" ] && /bin/cat "$USER_HOME/.systemd-env" | xargs printf ' %q')
    fi
    echo "Existential crisis"
    exit 1
fi
```
Relevant are the lines containing `SYSTEMD_EXE="/lib/systemd/systemd --system-unit=multi-user.target"` for which the original systemd fix from the repository did not consider a difference between WSL2 and Windows user names.
