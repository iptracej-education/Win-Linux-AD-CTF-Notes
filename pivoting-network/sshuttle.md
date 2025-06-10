# sshuttle

```bash
#install sshuttle
sudo apt-get install sshuttle -y

#connect to 10.1.1.0/24 segment over ssh via 10.11.1.251
sshuttle -r sean@10.11.1.251 10.1.1.0/24

-r: remote  [USERNAME[:PASSWORD]@]ADDR[:PORT]
-v: verbose 
-dns: capture local DNS requests and forward to the remote DNS server 

proxychains nmap -sTU --top-ports 10.1.1.27
```
