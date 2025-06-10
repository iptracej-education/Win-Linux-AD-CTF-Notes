---
description: Check shares with password, NTLM, and tickets
---

# Enum SMB Shares

### With/Without password

```bash
# Check if you have any access
crackmapexec smb $RHOST
crackmapexec smb $RHOST --shares
crackmapexec smb $RHOST -u '' -p '' 
crackmapexec smb $RHOST -u '' -p '' --shares
crackmapexec smb $RHOST -u 'anyuser' -p '' --shares

# List share
smbmap -H $RHOST -P 445 2>&1
smbmap -u null -p "" -H $RHOST -P 445 2>&1
smbclient -L //$RHOST 
smbclient -N -L //$RHOST 
smbclient -L \\\\$RHOST
smbclient -N -L \\\\$RHOST

# No password and login 
smbclient //$RHOST/apps
smbclient -N //$RHOST/apps
smbclient \\\\$RHOST\\apps
smbclient -N \\\\$RHOST\\apps

smbclient -N \\\\$RHOST\\profiles$  
smbclient -N \\\\$RHOST\\profiles\$   # Fish shell   

# When password available 
smbclient \\\\$RHOST\\forensic -U audit2020 
smbclient //$RHOST/apps -U baby2/library      # Prompt Password   
smbclient \\\\$RHOST\\apps -U baby2/library   # Prompt Password

# Try these directories when listed - logon scripts, GPO  
# Common share names for windows targets are
# C$
# D$
# ADMIN$
# IPC$
# PRINT$
# FAX$
# SYSVOL
# NETLOGON

smbclient \\\\$RHOST\\ADMIN$ # -U baby2/library if password available 
smbclient \\\\$RHOST\\IPC$ 
smbclient \\\\$RHOST\\NETLOGON  
smbclient \\\\$RHOST\\SYSVOL  

# Impacket-smbclient 
impacket-smbclient craft2/thecybergeek:winniethepooh@192.168.218.188
shares
use WebApp
put shell.php

# get all files 
smbget -R smb://$RHOST/share/

# Recursive via oneliner
smbclient  '\\\\$RHOST\\forensic' -U audit2020 -c 'mask "";prompt OFF;recurse ON; mget *'

# Recursive via manual command 
smb: \> mask ""
smb: \> recurse ON
smb: \> prompt OFF
smb: \> mget *

# Another recursive
smbget -U r.thompson -R smb://$RHOST/data/IT

```

### With Kerberos tickets

```bash
impacket-getTGT 'absolute.htb/svc_smb:AbsoluteSMBService123!' -dc-ip $RHOST
export KRB5CCNAME=svc_smb.ccache
impacket-smbclient -k -no-pass -dc-ip dc.absolute.htb 'dc.absolute.htb'
```

