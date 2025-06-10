---
description: >-
  This is the best practice to check any response on open ports. It sometimes
  unveils the hidden info for the services running for the ports.
---

# Q check on open port response

```bash
# automate
qportres nmap/all.nmap

# Manual
nc -w 3 $RHOST <port number>
```
