# Redis

### Connectivity

```bash
# Connection
nc -nv <Target IP> 6379

echo "Hello" 

# Enumeration 
```

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

### Enumeration

```bash
# Connection
redis-cli -h <Target IP>
redis-cli -h 192.168.90.69

# See configuration and quit
192.168.90.69:6379> config get *
192.168.90.69:6379> info
192.168.90.69:6379> quit
```

<figure><img src="../../.gitbook/assets/image (77).png" alt="" width="314"><figcaption></figcaption></figure>
