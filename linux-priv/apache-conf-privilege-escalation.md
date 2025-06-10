# Apache Conf Privilege Escalation

{% embed url="https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/apache-conf-privilege-escalation/" %}

### Key Assumptions:

1. Configuration file can be updated by current user.&#x20;

```bash
/ls -al /etc/apache2
```

1. Reverse shell script can be writeable to the web site directory.  Assume the website uses PHP, so we can create “shell.php” in the web root and insert [PHP reverse shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) script.
2. Apache can be restarted.&#x20;

```bash
# Debian and Ubuntu
/etc/init.d/apache2 restart
sudo /etc/init.d/apache2 restart 
sudo service apache2 restart
```

{% embed url="https://www.cyberciti.biz/faq/star-stop-restart-apache2-webserver/" %}
