# Recursive Download

### SMB

```bash
# With smbclient, 
mkdir download; cd download
smbclient //<IP>/Share -U <username> 
smb: \> mask ""
smb: \> recurse ON
smb: \> prompt OFF
smb: \> mget *

# With smbget,
mkdir download; cd download
smbget -R smb://10.10.10.143/share/
find all files and sort them by size
find . -type f  -exec du -h {} + | sort -h

```

### HTTP

{% code overflow="wrap" %}
```bash
# HTTP
wget -e robots=off --cut-dirs=3 --user-agent=Mozilla/5.0 --reject="index.html*" --no-parent --recursive --relative --level=1 --no-directories http://www.example.com/archive/example/5.3.0/
```
{% endcode %}

### FTP

{% code overflow="wrap" %}
```bash
# FTP
> bash
wget -r ftp://username:password@1.2.3.4/dir/*
	
wget -r ftp://1.2.3.4/dir/* --ftp-user=username --ftp-password=password
wget -r -l 0 ftp://192.168.142.219/* --ftp-user=stinky --ftp-password=wedgie57	
# -r,  --recursive                 specify recursive download
# -l,  --level=NUMBER              maximum recursion depth (inf or 0 for infinite)
	
wget -r --user="anonymous" --password="" ftp://192.168.142.208/
	
#IF you are stuck with PASSIVE mode, use no passive option in the following:
wget -r --no-passive --user="anonymous" --password="" ftp://192.168.146.65/

#TFTP
# create a list of files to download
cat ftp/upload/directory | awk '/[0-9] /{print $9}' > patrick.txt
# vi patcik.txt and remove . and ..
for file in `cat patrick.txt`; do echo -e "get $file\nquit\n"|tftp 192.168.142.209 36969; done

#LFTP
cd ftp && lftp -u anonymous,anonymous -e 'mirror;quit' 192.168.142.209; cd -

# Ncftp
# NcFTP is an FTP client program which debuted in 1991 as the first alternative FTP client
ncftp 10.11.1.226
get -R *
```
{% endcode %}

### CP & RCP

```bash
# cp
cp -r

# rcp
rcp -r documents zeus.univ.edu:backups 
```
