# Port 69 - UDP/TFTP

### Enumeration

```
nmap -p69 --script=tftp-enum.nse 10.11.1.111
```

### Command

{% code overflow="wrap" %}
```bash
# Connection
tftp 10.10.10.10
tftp 10.10.10.10 11111

# Status
tftp> status

# Set verbose output
tftp>verbose

# Set ASCII
tftp>ascii

# Set binary
tftp>binary

# Download a file
tftp> get <file>
tftp> mget *      # Download everything 

# Recursive File Download
## Example
cat ftp/upload/directory | awk '/[0-9] /{print $9}' > files.txt
for file in `cat files.txt`; do echo -e "get $file\nquit\n"|tftp 192.168.142.209 36969; done
 
```
{% endcode %}

### Vulnerable Version

```
Vuln tftp server 1.3, 1.4, 1.9, 2.1, and a few more
```
