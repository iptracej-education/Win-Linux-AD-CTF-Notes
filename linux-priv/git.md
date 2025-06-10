# Git

### Check logs&#x20;

{% code overflow="wrap" %}
```bash
git log
```
{% endcode %}

### Check Configs

```
git config -l
```

CTF: This may provide a username and password to login to the git repository to find something useful such as script or application to download  and execute, for example.&#x20;

### .git Enumeration&#x20;

```bash
# this will discover .git files, etc. 
/usr/share/seclists/Discovery/Web-Content/quickhits.txt
```

### gitdumper

{% code overflow="wrap" %}
```bash
# https://github.com/internetwache/GitTools 
./gitdumper.sh http://source.cereal.htb/.git/ source/
```
{% endcode %}

### Git Commands

```bash
# Go back to where it was at the last commit, restoring all the files
git reset --hard
```
