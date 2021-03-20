# Install terminator (Connector to XServer, inside Ubuntu)
* Install terminator  
  `apt-get install terminator`
* To verify installation run  
  `DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0 terminator &`  
  (should open terminator window in windows)

[Instruction source](https://medium.com/javarevisited/using-wsl-2-with-x-server-linux-on-windows-a372263533c3)
