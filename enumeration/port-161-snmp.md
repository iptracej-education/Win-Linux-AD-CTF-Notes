# Port 161 - SNMP

## Quick Intro

The Simple Network Management Protocol (SNMP) is a protocol **used in TCP/IP networks to collect and manage information about networked devices.** SNMP operates in the application layer (layer 7 of the OSI model) and uses **UDP port 161** to listen for requests. The SNMP protocol is supported by many types of devices including routers, switches, servers, printers, Network Attached Storage (NAS), firewalls, WLAN controllers and more. In the following sections we will be looking at the main components of SNMP managed networks, how they communicate with each other and something called the Management Information Base (MIB). We will also look at how and why SNMP can cause security issues and, of course, how to enumerate the SNMP protocol.

* SNMP is based on UDP, a simple, stateless protocol, and is therefore susceptible to IP spoofing, and replay attacks.
* In addition, the commonly used SNMP protocols 1, 2, and 2c offer no traffic encryption, meaning SNMP information and credentials can be easily intercepted over a local network.
* For all these reasons, SNMP is another of our favorite enumeration protocols.

#### Scanning for SNMP

{% code overflow="wrap" %}
```bash
# nmap 

> nmap -sU --open -p 161 192.168.11.200-254 -oG mega-snmp.txt
# -sU :: UDP scan
```
{% endcode %}

{% code overflow="wrap" %}
```bash
# onesixtyone

> echo public > community
> echo private >> community
> echo manager >> community
> for ip in $(seq 200 254);do echo 192.168.11.$ip;done > ips
> onesixtyone -c community i ips
```
{% endcode %}

{% code overflow="wrap" %}
```bash
# Snmpwalk 
snmpwalk -c public -v1 $RHOST
snmpwalk -c public -v2c $RHOST 

for community in public private manager; do snmpwalk -c $community -v1 $RHOST; done

# For Windows machines, there are interesting OIDs. 
1.3.6.1.2.1.25.1.6.0        System Processes
1.3.6.1.2.1.25.4.2.1.2     Running Programs
1.3.6.1.2.1.25.4.2.1.4     Processes Path
1.3.6.1.2.1.25.2.3.1.4     Storage Units
1.3.6.1.2.1.25.6.3.1.2     Software Name
1.3.6.1.4.1.77.1.2.25      User Accounts
1.3.6.1.2.1.6.13.1.3        TCP LocalPorts

# Enumerating the Entire MIB Tree
> snmpwalk  c public -v1 192.168.11.219

# Enumerating Windows Users:
> snmpwalk -c public -v1 192.168.11.204 1.3.6.1.4.1.77.1.2.25

# Enumerating Running Windows Processes:
> snmpwalk -c public -v1 192.168.11.204 1.3.6.1.2.1.25.4.2.1.2

# Enumerating Open TCP Ports:
> snmpwalk -c public -v1 192.168.11.204 1.3.6.1.2.1.6.13.1.3

# Enumerating Installed Software:
> snmpwalk -c public v1 192.168.11.204 1.3.6.1.2.1.25.6.3.1.2
```
{% endcode %}

\
