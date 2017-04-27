# custom_browser_handler
Open link using custom windows applications

https://msdn.microsoft.com/en-us/library/aa767914(v=vs.85).aspx

- Set registry
  Example of registry:
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\localbrowser]
@="URL:localbrowser Protocol"
"URL Protocol"=""

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\localbrowser\DefaultIcon]
@="C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe,1"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\localbrowser\shell]

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\localbrowser\shell\open]

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\localbrowser\shell\open\command]
@="wscript.exe \"C:\\Users\\simple\\Desktop\\invis.vbs\" \"C:\\Users\\simple\\Desktop\\localbrowser.bat\" \"%1\""
```

- Create handler
  - invis.vbs
```
set args = WScript.Arguments
num = args.Count

if num = 0 then
    WScript.Echo "Usage: [CScript | WScript] invis.vbs aScript.bat <some script arguments>"
    WScript.Quit 1
end if

sargs = ""
if num > 1 then
    sargs = " "
    for k = 1 to num - 1
        anArg = args.Item(k)
        sargs = sargs & anArg & " "
    next
end if

Set WshShell = WScript.CreateObject("WScript.Shell")

WshShell.Run """" & WScript.Arguments(0) & """" & sargs, 0, False
```
  - localbrowser.bat
```
set parameter=%1
set url=%parameter:localbrowser:=%
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" "%url%"
```
- Create link
```
<a href="localbrowser:https://mail.ru"> click local browser</a>
```
https://rawgit.com/flamencist/custom_browser_handler/master/example.html

https://jsfiddle.net/9vus2pnd/7/
