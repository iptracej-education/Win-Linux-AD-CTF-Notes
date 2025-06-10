# Win - Local Passwords/Files

### Search&#x20;

{% code overflow="wrap" %}
```bash
# Windows CMD
CMD> findstr /si password *.txt *.ini *.config 
CMD> findstr /SI "passw pwd" *.xml *.ini *.txt *.ps1 *.bat *.config 

# File Names and File Contents
CMD> dir /s *pass* == *cred* == *vnc* == *.config*
CMD> dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*
CMD> where /R C:\ user.txt
CMD> where /R C:\ *.ini

CMD> dir                                              #List current dir
CMD> dir /a:h C:\path\to\dir           #List hidden files
CMD> dir /s /b                                    #Recursive list without shit     
CMD> dir /s /b *pass*                       #List files that contains "pass" word in the filename
	
CMD> findstr /si password *.txt
CMD> findstr /si password *.xml
CMD> findstr /si password *.ini
	
# Find all passwords in all files.
CMD> findstr /spin "password" *.*
CMD> findstr /spin "password" *.*

# Powrshell
PS> Select-String -Path .\*.* -Pattern 'pass','cred','pwd' -SimpleMatch
PS> Get-ChildItem -Recurse | Where-Object { ! $_.PSIsContainer } | Select-String -Pattern 'pass','cred','pwd' -SimpleMatch
```
{% endcode %}

### Recycle Bin Hunting

```bash
dir C:\$Recycle.Bin /s /b
```

### Hidden Files

```bash
attrib -s -h -r /s /d *.*

# /a switch tp check hidden folders and files
dir /a C:\ 
```

### **Unattend.xml**

```
C:\unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.xml
C:\Windows\system32\sysprep\sysprep.xml
```

winPEAS can capture the info.&#x20;

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Kali, decode the value of the password in Unattend.xml
echo 'TABvAGMAYQBsAEEAZABtAGkAbgBQAGEAcwBzAHcAbwByAGQAMQAhAA==' | base64 --decode
```
{% endcode %}

### **PowerShell History File**

{% code overflow="wrap" %}
```bash
CMD> type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
PS> cat (Get-PSReadlineOption).HistorySavePath
```
{% endcode %}

winPEAS will capture the info. We will need to manually extract the contents of the file

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

### **IIS Config and Web Files**

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
CMD> type C:\inetpub\wwwroot\web.config
CMD> type C:\inetpub\wwwroot\conntectionstrings.config

# C:\intepub C:\apache C:\xampp 
PS> Get-Childitem -Recurse C:\inetpub | findstr -i "directory config txt aspx ps1 bat xml pass user"
PS> Get-Childitem -Recurse C:\apache | findstr -i "directory config txt php ps1 bat xml pass user"
PS> Get-Childitem -Recurse C:\xampp | findstr -i "directory config txt php ps1 bat xml pass user"
```
{% endcode %}

### Alternative Data Streams

Files have a primary data stream, which is what we normally see, for example a TXT file with some text inside. However, when a file is placed within another file, the data stream of the second files contents are considered alternate.&#x20;

```bash
# /R switch
dir /R
```

### **Stored Credentials (Credential Manager)**

```bash
cmdkey /list
```

<figure><img src="../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

### **Registry Keys**

{% code overflow="wrap" %}
```bash
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"
reg query "HKCU\Software\ORL\WinVNC3\Password"
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\WinVNC4" /v password
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

### **Hunting for SAM and SYSTEM Backups**

{% code overflow="wrap" %}
```bash
# Windows
# Check if SAM files are discovered. 
CMD> cd C:\ & dir /S /B SAM == SYSTEM == SAM.OLD == SYSTEM.OLD == SAM.BAK == SYSTEM.BAK

# Check if you can copy them
CMD> icacls "C:\Windows\System32\Config\Regback"

# Crack the SAM files 
kali> secretsdump.py -sam SAM.OLD -system SYSTEM.OLD LOCAL
```
{% endcode %}

Check (M) or (F) permission to modify or Full access.&#x20;

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

