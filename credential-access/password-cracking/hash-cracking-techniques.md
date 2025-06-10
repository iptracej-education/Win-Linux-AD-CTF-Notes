# Hash Cracking Techniques

### Local tools&#x20;

{% code overflow="wrap" %}
```bash
# Find the type of hash
hash-identifier 
https://hashes.com/en/tools/hash_identifier 

# John - Default 
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt --fork=8 

# John supported hash types
john --list=format

# John with a specific mode / hash type 
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt --fork=8 --format=crypt

# Hashcat auto mode - Default 
hashcat hashes.txt /usr/share/wordlists/rockyou.txt

# Hashcat supported hash types
hashcat --help | grep <hash type> 
hashcat --help | grep bcrypt

# Hashcat with a specific mode
hashcat -m 1400 hashes.txt /usr/share/wordlists/rockyou.txt 

# Crack /etc/shadow 
unshadow password.txt shadow.txt > unshadowed.txt; john --wordlist=<any word list> unshadowed.txt
```
{% endcode %}

### Hashcat Rules

{% embed url="https://in.security/2023/01/10/oneruletorulethemstill-new-and-improved/" %}

[https://github.com/stealthsploit/OneRuleToRuleThemStill](https://github.com/stealthsploit/OneRuleToRuleThemStill)

{% code overflow="wrap" %}
```bash
# 
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt -r /opt/passcrack/OneRuleToRuleThemStill/OneRuleToRuleThemStill.rule --debug-mode=1 --debug-file=matched.rule 

hashcat -a 0 -m 1000 cred/DC01-hashes.txt /usr/share/wordlists/rockyou.txt  -r /opt/passcrack/OneRuleToRuleThemStill/OneRuleToRuleThemStill.rule --debug-mode=1 --debug-file=matched.rule
```
{% endcode %}

### Online Rainbow Tools

```
https://crackstation.net/
http://www.cmd5.org/
https://hashkiller.co.uk/md5-decrypter.aspx
https://www.onlinehashcrack.com/
http://rainbowtables.it64.com/
http://www.md5online.org/
```
