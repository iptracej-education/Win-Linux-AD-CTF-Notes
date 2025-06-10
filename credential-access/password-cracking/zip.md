# Zip



```bash
# fcrackzip
# https://www.kali.org/tools/fcrackzip/
fcrackzip -u -D -p '/usr/share/wordlists/rockyou.txt' chall.zip

# zip2john and john
zip2john file.zip > zip.john
john zip.john


# hashcat
zip2john Zipfile.zip | cut -d ':' -f 2 > hashes.txt
hashcat -a 0 -m 13600 hashes.txt /usr/share/wordlists/rockyou.txt
```
