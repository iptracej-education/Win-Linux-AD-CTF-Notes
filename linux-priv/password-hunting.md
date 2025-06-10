# Password Hunting

### Password Hunting

```bash
# Tool based
./LinEnum.sh -t -k password

locate password

# Check hidden files
less .bash_history

# Check password keyword unders specific directories
grep pass /<login directory> /home /var/ opt/ <other obvious directory>
grep PASS /<login directory> /home /var/ opt/ <other obvious directory>

# Check filenames  
find / -name "*pass*" 2>/dev/null  
find / -name "*pass*" 2>/dev/null  -exec cp  {} /tmp/password/<directory> \;

# Look for something=something
grep -irE '(PASSWORD|password|pwd|pass)[[:space:]]*=[[:space:]]*[[:alpha:]]+' * 

# ID=sa;Password=CrimsonQuiltScalp193;

```

<figure><img src="../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

