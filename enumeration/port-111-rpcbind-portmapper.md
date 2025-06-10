# Port 111 - Rpcbind/portmapper

Port 111 is associated with the Remote Procedure Call (RPC) protocol's portmapper service. The RPC portmapper service, often referred to as "rpcbind" in modern implementations.

### Enumeration

{% code overflow="wrap" %}
```bash
# Check if rpcbind running in the subnet
nmap -sV -p111 --script=rpcinfo 10.11.1.1-254

# Check if rpcbind running on the box
nmap -sSUC -p111 192.168.10.1

# Login to the port
rpcclient -U "" 10.11.1.111
	srvinfo
	enumdomusers
	getdompwinfo
	querydominfo
	netshareenum
	netshareenumall
	querydispinfo

# rpcbind + NFS
nmap -p 111 --script nfs* 10.11.1.72

rpcinfo -p 10.11.1.111  # enum NFS shares
showmount -e 10.11.1.111 # show if we can mount

```
{% endcode %}

### NFS Share Mount

See [Port 2049 - NFS](https://app.gitbook.com/o/sbYQpaBkV0ueQjvLIdTt/s/nA4bAkddGXesk1QCLYAY/~/changes/187/enumeration/port-2049-nfs) section.

{% code overflow="wrap" %}
```bash
# Check more Port 2049 - NFS

sudo mount -t nfs 10.11.1.111:/ /mnt -o nolock  # mount remote share to local machine
sudo mount -t nfs -o nfsvers=3 10.11.1.72:/home /mnt/ # version3
sudo mount -t nfs -o nfsvers=3 10.11.1.72:/home ~/oscp/lab/10.11.1.72/home/
# -o: options
# -t: types

# If you mount a folder which contains files or folders only accesible by some user (by UID). You can create locally a user with that UID and using that user you will be able to access the file/folder. For example, the folder permission looks like below.

# drwxr-xr-x    2    1014 1014 4096    home
# drwxr-xr-x    2    1014 1014 4096    folder1

cd home/ && ls
sudo adduser pwn
sudo sed -i -e 's/1001/1014/g' /etc/passwd
cat /etc/passwd | grep pwn
su pwn
```
{% endcode %}

