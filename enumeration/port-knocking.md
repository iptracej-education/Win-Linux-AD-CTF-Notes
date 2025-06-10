# Port Knocking

### Discovery

{% code overflow="wrap" %}
```bash
# You find a port knocking configuration in SMB share or other locations.
# ./private/opensesame/config, 
# /etc/knockd.conf, etc.
[openHTTP] 
    sequence     = 159,27391,4
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

### Knocking

{% code overflow="wrap" %}
```bash
# Oneliner
for port in 159 27391 4; do nmap -Pn --host_timeout 201 --max-retries 0 -p $port $RHOST; done

# Separate commadns 
nc -v $RHOST 159
nc -v $RHOST 27391
nc -v $RHOST 4
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

### Enumeration again

```bash
nmap -sC -sV $RHOST
```
