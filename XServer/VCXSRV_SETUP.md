# XServer Windows)
## Install VcXsrv
* [Download VcXsrv](https://sourceforge.net/projects/vcxsrv/)
* Follow installation instructions
  * **IMPORTANT: Be sure to allow private and public network connections through firewall**
* Create desktop shortcut for VcXsrv
* Edit shortcut target with `{{target}} :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl -dpi auto`
  * '{{target}}' is the path to `vsxsrv.exe`
  * This changes the startup options to your needs
* Start shortcut
* Add shortcut to autostart  
  run `shell:startup` to open autostart and copy shortcut here
* Verify in WSL2  
  `netstat -abno|findstr 6000`

[Instruction source](https://medium.com/javarevisited/using-wsl-2-with-x-server-linux-on-windows-a372263533c3)
