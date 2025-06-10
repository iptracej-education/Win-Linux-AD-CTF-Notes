# Port 139/445 - SMB

### Enumeration

```bash
enum4linux -a -l 10.10.10.143
enum4linux-ng.py -A -C -v 10.10.10.143
nmap --script "safe or smb-enum-*" -p 445 10.10.10.143
nmap -v --script=smb-enum* -p T:139,445 10.10.10.143
nmap -v --script=smb-vuln* -p T:139,445 10.10.10.143
```

### List Shares

{% code overflow="wrap" %}
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

# Access to the share
smbclient //$RHOST/share/ -N -L          

# No password and login 
smbclient  '\\\\$RHOST\profiles$' -N  

# if password available 
smbclient  '\\\\$RHOST\\forensic' -U audit2020  

# For older SMB protocol
smbclient -N //10.10.10.3/tmp --option='client min protocol=NT1'

# Impacket-smbclient 
impacket-smbclient craft2/thecybergeek:winniethepooh@192.168.218.188
shares
use WebApp
put shell.php

# get all files and sort it
mkdir download; cd download
smbget -R smb://$RHOST/share/
find . -type f  -exec du -h {} + | sort -h

# Recursive via oneliner
smbclient  '\\\\$RHOST\\forensic' -U audit2020 -c 'mask "";prompt OFF;recurse ON; mget *'

# Recursive via manual command 
smb: \> mask ""
smb: \> recurse ON
smb: \> prompt OFF
smb: \> mget *

# Another recursive
smbget -U r.thompson -R smb://$RHOST/data/IT

# Read all files
for file in $(find . -type f); do echo ">> $file <<" && cat $file; done # Include .(dot)file and recursive
for i in *; do echo ">> $i <<" && cat $i; done # No .(dot) file and not recursive
```
{% endcode %}

### Samba rpcclient

```bash
rpcclient -U "" -N 10.10.10.143  # -U:Username -N:No-pass
rpcclient -U "usernmae" 10.10.10.143
password:"password"
rpcclient $> enumdomusers
rpcclient $> enumprinters
rpcclient $> enum
rpcclient $> querydominfo
```





