---
description: >-
  This section lists AD user recon techniques for common ports. Finding domain
  users and their credentials is the most important part of the Active Directory
  penetration testing.
---

# User Recon

### RCP (139, 445) [MS-RPC - The Hacker Recipes](https://www.thehacker.recipes/ad/recon/ms-rpc)

{% code overflow="wrap" %}
```bash
# No password
rpcclient -U '' -N $RHOST

# rpcclient userful commands to enumerate users and groups 
>enumdomusers
>enumdomgroups
>querydispinfo
>querydominfo 

# With password
rpcclient -U Throwback.local/BlaireJ 10.200.74.117

# Create a usernames.txt
vi enumdomusers.txt
# Copy and paste it to the enumdomusers.txt

cat enumdomusers.txt | cut -d ':' -f 2 | cut -d '[' -f 2 | cut -d ']' -f 1 > usernames.txt
```
{% endcode %}

### SMB (139,445) [enum4linux ⚙️ - The Hacker Recipes](https://www.thehacker.recipes/ad/recon/enum4linux)

{% code overflow="wrap" %}
```bash
# Enumeratae AD machine 
enum4linux $RHOST 
enum4linux-ng.py -A $RHOST

# Check if you can collect the usernames
crackmapexec smb $RHOST --users
crackmapexec smb $RHOST -u '' -p '' --users 
crackmapexec smb $RHOST -u 'anyuser' -p '' --users

# Check if you can enumerate users by bruteforcing the RID on the remote target
cme smb rebound.htb -u guest -p '' --shares --rid-brute 10000

# With username and password
cme smb $RHOST -u svc-alfresco -p s3rvice --shares

```
{% endcode %}

### SMB (139,445) - Domain Users

{% code overflow="wrap" %}
```bash
net rpc group members 'Domain Users' -W 'NORTH' -I $RHOST -U '%'
```
{% endcode %}

### Kerberos (88)

{% code overflow="wrap" %}
```bash
kerbrute userenum /usr/share/seclists/Usernames/Names/names.txt -d <domain name> --dc $RHOST 
	
# Large list of username only if you cannot find any users
kerbrute userenum /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -d <domain name> --dc $RHOST 
```
{% endcode %}

See Kerberos attacks section for attack techniques.

### LDAP TCP (389)

{% code overflow="wrap" %}
```bash
# Search ldap based information. You might find some juicy info. 
ldapsearch -H ldap://$RHOST -x -s base namingcontexts
ldapsearch -H ldap://$RHOST -x -b " DC=DANTE,DC=local"
ldapsearch -H ldap://$RHOST -D '' -w '' -b "dc=dante,dc=local"
ldapsearch -H ldap://$RHOST -D '' -w '' -b "dc=cascade,dc=local" | grep -iE '(password|pwd|pass|info|desc)'

# Check if you can use username and password
cme ldap $RHOST -u ldap -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'
# Search all objects 
ldapsearch -H ldap://support.htb -D ldap@support.htb -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "dc=support,dc=htb" "*"

# In case of Additional Security Error (AcceptSecurityContext error, etc.), may add -Y option with GSSAPI. 
ldapsearch -H ldap://dc.absolute.htb -Y GSSAPI -b "cn=users,dc=absolute,dc=htb" users description
```
{% endcode %}

### LDAP TCP (389) - User Description

{% code overflow="wrap" %}
```bash
# Query to get user descrition via cme ldap and get-desc-users module
cme ldap 192.168.56.11 -d north.sevenkingdoms.local -u brandon.stark -p iseedeadpeople -M get-desc-users
```
{% endcode %}

### SSL (443)

```bash
Check certificate with common name/issuer name 
```





