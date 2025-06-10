# Common Tips

1. Establish your manual privilege escalation enumeration.
2. Practice them until you can run the commands without copy and paste them.
3. Run the quick win commands first.&#x20;

{% code overflow="wrap" %}
```bash
# any SUDO command to run 
Sudo -l 

# SUID bit to run
find / -perm -u=s -type f 2>/dev/null

# Check environmental variables
env 

# Any juicy information in files under a directory 
find -type f -exec batcat {} +    # especially under /home/<user> directory. 
```
{% endcode %}

1. Check the date of logged-in machine OS installed and then the files/directories changed so you may be able to find where the latest files and configurations are touched by admins.&#x20;
2. Do not use local enumeration tools until you complete manual local enumeration or run only when you need another perspective for privilege escalation. There is much information that will be captured, so you may lose your focus or fail to identify intended ways of solving the machines. For certification pursuer, do not expect that such tools will capture all vulnerabilities that you could try.&#x20;
   1. linpeas.sh
   2. lse.sh&#x20;
   3. LinEnum.sh
3. Do not forget enumerating all files in your first logged-in directory!
4. Check unusual directories and files under /, /tmp, /opt, /var, and /home, and /home/username.&#x20;
5. Do not forget checking dot files under /home/username directory!&#x20;
6. Do not forget enumerating all password files (_pass_, creds) in first login directory, home, var/www, var/log, /opt/, and /tmp directory.
7. Creds can be hidden in files / database / configurations as well as images and binaries!&#x20;
8. Enumerate running programs and services. Identify the version numbers of the programs and services that might have vulnerabilities.&#x20;
9. You need to be comfortable to use network pivoting techniques to connect from remote machine.&#x20;

```bash
# Local port forwarding
ssh -f -N -L 2049:127.0.0.1:2049 megan@x.x.x.x
-f: run as background
-N: no command executed on remote server
-L: Local port forwarding # -L local port:remote host:remote port
```

11. Think through weak configurations and misconfigurations occurred in current machine environment & programs and services running.&#x20;

* Understand the local database enumeration methodology.
* Understand /etc/ directory and where weak configurations might occur&#x20;
* Understand dockers and any weaknesses
* Understand management tools like Ansible and YAML misconfigurations

&#x20;etc.&#x20;

12. Run **pspy** and observe the processes running constantly and see anything run by root or next privilege escalation user targets.  [https://github.com/DominicBreuker/pspy](https://github.com/DominicBreuker/pspy)

