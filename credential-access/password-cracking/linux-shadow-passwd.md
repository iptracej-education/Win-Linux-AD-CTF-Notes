# Linux shadow passwd



{% code overflow="wrap" %}
```bash
# Linux shadow passwd
# copy the two files and save them as passwd.txt and shadow.txt respectively.

unshadow passwd.txt shadow.txt > unshadowed.txt
john --rules --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt

# hashcat 
unshadow passwd.txt shadow.txt > unshadowed.txt
hashcat unshadowed.txt /usr/share/wordlists/rockyou.txt
```
{% endcode %}
