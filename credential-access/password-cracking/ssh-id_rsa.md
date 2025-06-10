# SSH id\_rsa



```bash
# SSH id_rsa to crack

ssh2john id_rsa > id_rsa.hash
john --wordlist=darkweb2017-top10.txt id_rsa.hash
```
