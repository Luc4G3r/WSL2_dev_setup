# Install terminator (Connector to XServer, inside Ubuntu)
* Install terminator  
  `apt-get install terminator`
* To verify installation run  
  `DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0 terminator &`  
  (should open terminator window in windows)
## Hook to terminator startup (from windows)
* create `C:\Windows\System32\wscript.exe C:\Users\<YOUR_USER>\linux\terminator\startTerminator.vbs`  
  content:  
  ``
    args = "-c" & " -l " & """DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0 terminator"""
    WScript.CreateObject("Shell.Application").ShellExecute "bash", args, "", "open", 0
  ``
* create shortcut to `startTerminator.vbs` to open faster
