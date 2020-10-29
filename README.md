# WSL2_dev_setup
Setup instructions WSL2 for web development (XServer and Windows GUI variants)

## WSL2, Ubuntu 20 LTS installations
// TODO

## XServer
[Instruction source](https://medium.com/javarevisited/using-wsl-2-with-x-server-linux-on-windows-a372263533c3)
### Install VcXsrv
[Download VcXsrv](https://sourceforge.net/projects/vcxsrv/)
* Follow installation instructions
* Create desktop shortcut for VcXsrv
* Edit shortcut target with {{target}} :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl -dpi auto
* Start shortcut
* Add shortcut to autostart  
  run `shell:startup` to open autostart
* Verify  
  `netstat -abno|findstr 6000`
### Install terminator
* Install terminator on WSL2  
  `apt-get install terminator`
* Verify  
  `DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0 terminator &` should open terminator window
### Hook
* create `C:\Windows\System32\wscript.exe C:\Users\<YOUR_USER>\linux\terminator\startTerminator.vbs`  
  content:  
  ```
    args = "-c" & " -l " & """DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0 terminator"""
    WScript.CreateObject("Shell.Application").ShellExecute "bash", args, "", "open", 0
  ```
* create shortcut to `startTerminator.vbs`
### PHPStorm install WSL2
[Follow systemd installation instructions](https://github.com/DamionGans/ubuntu-wsl2-systemd-script)
* Install PHPStorm
  `snap install phpstorm --classic`

[More details](https://github.com/lackovic/notes/tree/master/Windows/Windows%20Subsystem%20for%20Linux#run-a-linux-gui-application-in-wsl-2)
