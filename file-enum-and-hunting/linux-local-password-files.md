# Linux - Local Password/Files

### Password Hunting

{% code overflow="wrap" %}
```bash
# Tool
./LinEnum.sh -t -k password

# Check any password file. 	
locate password
	
# Check hidden files
less .bash_history
	
# Craw pass keyword under the current directory
grep -irE '(password|pwd|pass)[[:space:]]*=[[:space:]]*[[:alpha:]]+' *
# This may discover something like 'ID=sa;Password=CrimsonQuiltScalp193;' 

# Craw pass keyword under specific directories
grep pass /<login directory> /home /var/ opt/ <other obvious directory>
grep PASS /<login directory> /home /var/ opt/ <other obvious directory>
	
# Do "creds" as well 
find / -name "*pass*" 2>/dev/null  
find / -name "*pass*" 2>/dev/null  -exec cp  {} /tmp/password/<directory> \;
find / -name <file> 2>/dev/null -exec grep pass {} \;
find / -name <file> 2>/dev/null -exect grep PASS {} \;
	
# Read all files
cd <directory>
for file in $(find . -type f); do echo ">> $file <<"; cat $file; done
	
# Craw "PASSWORD=" keyward under all directories 	
grep --color=auto -rnw '/' -ie "PASSWORD=" --color=always 2>/dev/null

# pspy and find some process info that may include password
./pspy

# Anything interesting
/var/spool/mail
less /var/mail/$(whoami)

# Anything interesting in web 
ls -alhR /var/www/
ls -alhR /srv/www/htdocs/
ls -alhR /usr/local/www/apache22/data/
ls -alhR /opt/lampp/htdocs/
ls -alhR /var/www/html/
ls -alhR /usr/local/tomcat 

# Anything interesting in log files
cat /etc/httpd/logs/access_log
cat /etc/httpd/logs/access.log
cat /etc/httpd/logs/error_log
cat /etc/httpd/logs/error.log
cat /var/log/apache2/access_log
cat /var/log/apache2/access.log
cat /var/log/apache2/error_log
cat /var/log/apache2/error.log
cat /var/log/apache/access_log
cat /var/log/apache/access.log
cat /var/log/auth.log
cat /var/log/chttp.log
cat /var/log/cups/error_log
cat /var/log/dpkg.log
cat /var/log/faillog
cat /var/log/httpd/access_log
cat /var/log/httpd/access.log
cat /var/log/httpd/error_log
cat /var/log/httpd/error.log
cat /var/log/lastlog
cat /var/log/lighttpd/access.log
cat /var/log/lighttpd/error.log
cat /var/log/lighttpd/lighttpd.access.log
cat /var/log/lighttpd/lighttpd.error.log
cat /var/log/messages
cat /var/log/secure
cat /var/log/syslog
cat /var/log/wtmp
cat /var/log/xferlog
cat /var/log/yum.log
cat /var/run/utmp
cat /var/webmin/miniserv.log
cat /var/www/logs/access_log
cat /var/www/logs/access.log

ls -alh /var/lib/dhcp3/
ls -alh /var/log/postgresql/
ls -alh /var/log/proftpd/
ls -alh /var/log/samba/

```
{% endcode %}
