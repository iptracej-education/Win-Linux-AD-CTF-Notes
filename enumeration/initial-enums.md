---
description: >-
  This will capture initial reconnaissance data from target network and hosts.
  We run the commands below for almost all CTF environments all the time.
---

# Initial enums

### IP Range

```bash
netdiscover -r 10.10.10.0/24 
nmap -sn 10.11.1.1-255 | grep "Nmap scan report for" | cut -d " " -f 5
nmap -sP -PR -n 10.11.1.1-254 | grep "Nmap scan report for" | cut -d" " -f5
```

### Rustscan

```bash
# Export RHOST=<IP Address>

rustscan $RHOST -t 500 -b 1500 -- -A
-t: the number of threads
-b: sets the rate at which packets are sen
--: to separate a command for nmap
-A: all in nmap option
```

### Nmap

```bash
nmap -sC -sV -oA nmap/init $RHOST
sudo nmap -p- -sV -vv --open --reason $RHOST
sudo nmap -vv -sC -sV -p- -oA nmap/all --max-retries 0 $RHOST

# OneTwoPunch
https://raw.githubusercontent.com/superkojiman/onetwopunch/master/onetwopunch.sh
onetwopunch.sh ip.txt tcp

# Scan for UDP
nmap 10.11.1.111 -sU
unicornscan -mU -v -I 10.11.1.111

# Connect to tcp/udp if one is open
nc 10.10.10.10 80
nc -u 10.11.1.111 48772
```

### nmapAutomator

```bash
sudo nmapAutomator -H $RHOST -t All -o nmapautomator/
```

### Autorecon

```bash
sudo -E su -p
autorecon.py -vv -o autorecon/ 10.10.10.143
/opt/enum/AutoRecon/autorecon.py -vv -o autorecon/ $RHOST  
```

### NmapAutomator

```bash
nmapAutomator.sh -H $RHOST -t All
```

### Sparta

```bash
# This may be run for assurance.
https://github.com/SECFORCE/sparta

/cd /opt/enum/sparta
python3 sparta.py
```

