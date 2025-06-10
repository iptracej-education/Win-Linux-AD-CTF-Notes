# SSH private key crack

```bash
# Convert key to john format
ssh2john <id_rsa> > key.txt

# Crack it with john
john key.txt --wordlist=/usr/share/wordlists/rockyou.txt --fork=8
```
