# Ping check

Ping connectivity testing from a target machine

```bash
Kali> sudo tcpdump -i tun0 -c5 icmp

# Target machine
CMD> ping -c 5 <IP Address>
CMD> ping <IP Address>
```
