# XServer Windows)
## Install VcXsrv
[Download VcXsrv](https://sourceforge.net/projects/vcxsrv/)
* Follow installation instructions
* Create desktop shortcut for VcXsrv
* Edit shortcut target with {{target}} :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl -dpi auto
* Start shortcut
* Add shortcut to autostart  
  run `shell:startup` to open autostart and copy shortcut here
* Verify  
  `netstat -abno|findstr 6000`

[Instruction source](https://medium.com/javarevisited/using-wsl-2-with-x-server-linux-on-windows-a372263533c3)
