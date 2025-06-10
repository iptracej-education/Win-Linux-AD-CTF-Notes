# First Recon

#### First recon with crackmapexec <a href="#first-recon-with-cme" id="first-recon-with-cme"></a>

```bash
# This is the best enumeration before firing up nmap
cme smb <IP range>
cme smb 192.168.56.1/24
```

#### Probe Domain Controllers

{% code overflow="wrap" %}
```bash
# Check if the targets open ports 389 (LDAP) and 88 (Kerberos). If yes, these are DC most of time. color_output is the aliases that calls sed command to color the output. 

nmap -p 389,88 192.168.56.0/24 --open -Pn | color_output
nmap -p 389,88 $RHOST --open -Pn | color_output
# alias color_output="sed -E -e 's/([0-9]{1,3}\.){3}[0-9]{1,3}/\x1b[32m&\x1b[0m/g' -e 's/([a-zA-Z0-9.-]+\.){1,}[a-zA-Z]{2,6}/\x1b[32m&\x1b[0m/g' -E -e 's/(open)/\x1b[33m&\x1b[0m/g'"
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

#### Nmap recon

{% code overflow="wrap" %}
```bash
# Assume that you edit ip.exe that contains IP addresses for targets
# All scan files are stored in nmap directory

# ARP scan
nmap -sP -PR -n 10.11.1.1-254 

# Ping Sweep (No port scan)
nmap -sn 192.168.56.0/24 | grep "Nmap scan report for" | cut -d" " -f5
nmap -sn 192.168.56.0/24 | grep "Nmap scan report for" | cut -d" " -f5 > findings/ip.txt

# Port scan
for i in $(cat ip.txt); do echo $i && sudo nmap -vv -sC -sV -p- -oA nmap/$i --max-retries 0 $i;done
```
{% endcode %}

#### Autorecon

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong># Assume ip.txt exists
</strong><strong># All scan files are stored in autorecon directory
</strong><strong>
</strong><strong>sudo -E su -p
</strong><strong>autorecon -vv o autorecon -t ip.txt
</strong></code></pre>

#### DNS recon

```bash
# dig @<dns server> <domain> axfr
# Assume that DC is DNS server 
dig @<DC ip address> yourdomain.htb axfr 

# Automated recons
dnsrecon -d yourdomain.htb
dnsrecon -d yourdomain.htb -a -n <IP_DNS>  # Zone transfer
dnsrecon -d sevenkingdoms.local -n 192.168.56.10

# git clone https://github.com/dirkjanm/adidnsdump
# cd adidnsdump
# pip3 install .
# mkdir yourproject/dns && cd yourproject/dns
# The tool will produce recodes.csv file locally
cd findings/domain
adidnsdump winterfell.north.sevenkingdoms.local 
```
