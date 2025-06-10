# Port 53 - DNS

### Enumeration

```bash
# Primary DNS check
host <domain name> 
host -t ns <domain name>
host -t mx <domain name> 

nslookup contoso.com

# Reverse DNS check
host <ip address> 

# DNS zone transfer file 
## host -l <domain name> <name server>
host -l googlecom ns1.google.com

## dig @<dns server> <domain> axfr
dig @10.10.10.123 friendzone.red axfr 

# Automated recons
dnsenum google.com 
dnsrecon -d contoso.com
dnsrecon -d active.htb -a -n <IP_DNS>  # Zone transfer
```

### Scripts

```bash
# if argument was given, identify the DNS servers for the domain
for server in $(host ­-t ns $1 | cut ­-d" " ­-f4);do
# For each of these servers, attempt a zone transfer
host -l $1 $server | grep "has address"
done
```

### Active Directory Server

```
dig -t _gc._tcp.lab.domain.com
dig -t _ldap._tcp.lab.domain.com
dig -t _kerberos._tcp.lab.domain.com
dig -t _kpasswd._tcp.lab.domain.com
nmap --script dns-srv-enum --script-args "dns-srv-enum.domain='domain.com'"
```

