# Port 80/443 - Web

### Web Directory Enumeration

{% code overflow="wrap" fullWidth="false" %}
```bash
# Export URL=<http(s)://FQDN>
feroxbuster -k -e -u "$URL" -x html txt php js zip bak xml log -t 200 -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt
feroxbuster -k -e -u "$URL" -x html txt php js zip bak xml log -t 200 -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt

# Windows 
feroxbuster -k -e -u "$URL" -x html txt asps asp htm zip bak xml log -t 200 -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt

```
{% endcode %}

### Web File Enumeration&#x20;

{% code overflow="wrap" %}
```bash
# Export URL=<http(s)://FQDN>/

feroxbuster -k -e -u "$URL" -x html txt php js zip bak xml log -t 200 -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt

# For some lenghy and complex directories and 
# n: no recursion
feroxbuster -e -u "$URL" -x html txt php js zip bak xml -t 200 -w /usr/share/seclists/Discovery/Web-Content/quickhits.txt --filter-status 401,402,403,404,500,501,502 --quiet -n

# Discover quickwin files and holders - GIT
feroxbuster -e -u "$URL" -x html txt php js zip bak xml -t 200 -w /usr/share/seclists/Discovery/Web-Content/quickhits.txt

# Windows
feroxbuster -e -u "$URL" -x html txt asps asp htm zip bak xml log -t 200 -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt
```
{% endcode %}

### Subdomain Enumeration

{% code overflow="wrap" %}
```bash
# Change the value of -fw option when running at the beginning

ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.gofer.htb" -u http://gofer.htb -fw 20

ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -H "Host: FUZZ.runner.htb" -u http://runner.htb -fw 4

# Bug Bounty Program - Real World Wordlist

https://github.com/trickest/wordlists/blob/main/inventory/subdomains.txt


```
{% endcode %}

### Parameter Enumeration

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"># GET
arjun -u http://server.com/page/

ffuf -ic -c -u "http://proxy.gofer.htb/index.php?FUZZ=file:/etc/passwd" -w "/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt" -t 200 -fw 42

wfuzz -z file,./burp-parameter-names.txt "http://satctrl.bahamas.ysh/action.php?FUZZ=aaaaaaa" 

# POST 
<strong>arjun -u http://server.com/page/ -m POST
</strong>
ffuf -X POST -ic -c -u "http://proxy.gofer.htb/index.php?FUZZ=/etc/passwd" -w "/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt" -t 200 -fw 9
</code></pre>
