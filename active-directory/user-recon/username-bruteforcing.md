---
description: >-
  This is useful when anonymous sessions are not allowed. Bruteforcing still is
  a valid technique.
---

# Username bruteforcing

### Discover username&#x20;

* wordlist collection&#x20;
* Web site info
* Image metadata&#x20;
* Other information collected from the other machines&#x20;
* ONIST&#x20;

### Create a username list

#### Manual creation

```bash
# Change all lower to all upper
tr '[:lower:]' '[:upper:]' < users.txt >> users-updated.txt

# Change all upper cases to all lower cases
tr '[:upper:]' '[:lower:]' < users.txt > users-updated.txt

# Change the first letter only to upper in the string
cat users.txt | sed 's/./\U&/'
```

#### username-anarchy

```bash
#Create an initial list of users
 James Roberts
 Michale Chaffrey
 Donald Klay
 Sarah Osvald
 Jeffer Robinson
 Nicole Smith

# Then run the username-anarchy to create different username combinations
username-anarchy -i authors -f flast,lfirst,f.last > got_users.txt
```

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

#### Brutefocing usernames against a Kerberos service

{% code overflow="wrap" %}
```bash
nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm='essos.local',userdb=got_users.txt" 192.168.56.12

nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm='sevenkingdoms.local',userdb=got_users.txt" 192.168.56.10
```
{% endcode %}

