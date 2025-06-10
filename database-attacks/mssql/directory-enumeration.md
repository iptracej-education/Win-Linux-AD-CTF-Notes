---
description: Use dirtree function in SQL to enumerate directory and file names.
---

# Directory Enumeration

### Common Commands

```bash
# Enumerate a directory under C:\
SQL> master.sys.xp_dirtree 'C:\',1,1
SQL> master.sys.xp_dirtree 'c:\inetpub\wwwroot',1,1;
```
