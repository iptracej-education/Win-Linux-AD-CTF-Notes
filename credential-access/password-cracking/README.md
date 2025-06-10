# Password cracking

### Online Password Hash Cracking

[_https://crackstation.net/_](https://crackstation.net/) [_http://finder.insidepro.com/_](http://finder.insidepro.com/)

### John the ripper

{% code overflow="wrap" %}
```bash
# Auto detection

john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
 
# JTR forced descrypt cracking with wordlist
john --format=descrypt hashes.txt

# MD5 hash
john --wordlist=/usr/share/wordlists/rockyou.txt -format=Raw-MD5 hashes.txt
```
{% endcode %}

### Hashcat

```bash
# Check the algorithm 
hashcat -h | grep sha512

# Run it with the mode number and wordlist
hashcat -m 1800 hash2.txt /usr/share/wordlists/rockyou.txt  

```

### Colabcat (Hashcat on Google Colab

Run [Hashcat](https://hashcat.net/) on [Google Colab](https://colab.research.google.com/) with session restore capabilities with Google Drive.

{% @github-files/github-code-block %}

### Penglab

Abuse of Google Colab for fun and profit.

[https://github.com/mxrch/penglab](https://github.com/mxrch/penglab)
