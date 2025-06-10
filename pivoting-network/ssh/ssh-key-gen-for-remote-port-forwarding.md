# SSH key gen for remote port forwarding

### Generate SSH key

{% code overflow="wrap" %}
```bash
# @Kali

mkdir /tmp/keys
cd /tmp/keys
	
ssh-keygen
# /tmp/keys/id_rsa 

cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCy5rYIrdAscz3pInnmmzrARamiDlRRm8pQGKuN8FuQIqbus+9y6D9Ek/rT/bOWoq1zShrcL4LVR2yijDNs0I8pFf3/jf/rxICEltPQhs/pNVXDzkGbm9WprOePZmMNCWGn0854BgbG5mX0PGRAVjCCjCTOP3t5waBUDLL1vPwLTjmuCdhIa0Xs6LGKiUviqz/IarxrzpnZelrESRPfByHzrf8J3ic+lIdbknUVo8PYU3Lw42p7LF0MKM1RkS4wWVT9YdnHaZgzX8XVq3FA1H0cHz8AMIwRlAn4b39xWSoOJ7i9p5Vz2+KP9eco105TPeyC3cR5HASxh6MVdF+X6njd www-data@ajla
```
{% endcode %}

### Adding the public key to Authorized\_Key file

{% code overflow="wrap" %}
```bash
# @Kali

systemctl start ssh
vi ~/.ssh/Authorized_keys

# Adding the following to the file
from="<Target IP>",command="echo 'This account can only be used for port forwarding'", no-agent-forwarding,no-X11-forwarding,no-pty ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDxKaOWuh6wQJRwI0/WEx6CxHyUoO5XL10IEq8Ikd0wXN3kIntVt1MEPZ0f1os5GsfXuvdriQ4jnfv8NbEcpsj34EHZ5IX3wBYj3F98hLPenNCrtQn2zfaki9ITOiYVwiMs7/ZIeY0zOtXQiQQcaw/h0IUgfwnnbbH/r64TrVdhMk53cHdDPjq9ON5v4TOvNasdEZoEcAUCROXid46gSUqUeBq3L9zGT3di3AkXc8fH8rnoGib5UC18msaG920iLLr3aUNevgu/eurPLjiX5rnuuQiaXFbR1IHaRbKVnMpMex7KB0Bu6OgRuicKewK7nLCRA3pgIxWBTloRGydAxnL www-data@ajla
```
{% endcode %}
