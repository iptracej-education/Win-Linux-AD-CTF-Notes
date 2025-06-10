---
description: KeePass is a free open-source password manager
---

# KeePass database

### Crack the password

{% code overflow="wrap" %}
```bash
sudo apt-get install -y kpcli #Install keepass tools like keepass2john

# For John
keepass2john Database.kdbx > Keepasshash.txt
# In case if you need a password file to access 
keepass2john -k <file-password> Database.kdbx > Keepasshash.txt

john --wordlist=/usr/share/wordlists/rockyou.txt Keepasshash.txt

# For hashcat
keepass2john CEH.kdbx | cut -d ':' -f 2 > jeeves.keepass # Remove the name of DB at the beggining

hashcat  -a 0 -m 13400 -O jeeves.keepass /usr/share/wordlists/rockyou.txt  
```
{% endcode %}

### Access to Database

```bash
# Install
sudo apt-get install kpcli libterm-readline-gnu-perl libdata-password-perl

# Access (Assume that you crack the passwrod and save it in keypass file. 
kpcli --pwfile=./keypass --kdb=CEH.kdbx    
kpcli:/> help  
kpcli:/> ls 
kpcli:/> cd CEH 
kpcli:/> ls 

# Show entry - change the number
kpcli:/CEH> show 0
kpcli:/CEH> show -f 0
```

