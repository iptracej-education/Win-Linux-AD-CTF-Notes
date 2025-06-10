# Linux shadow

{% code overflow="wrap" %}
```bash
# if the hash starts by $1, md5 is used
# if the hash starts by $2$ or $2a$, Blowfish is used;
# if the hash starts by $5$, SHA-256 is used;
# if the hash starts by $6$, SHA-512 is used.

# /etc/shadow hash
echo '$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVl aXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0' > hash.txt

# Auto-discovery mode
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

# For $1  
john --format=md5crypt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
# For $5  
john --format=sha256crypt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
# For $6
john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```
{% endcode %}
