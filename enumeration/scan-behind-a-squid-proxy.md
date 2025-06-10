# Scan behind a squid proxy



```bash
# Quick scan for http 
curl http://targetIP --proxy <proxy IP>:<port> 
```

[aancw/spose: Squid Pivoting Open Port Scanner (github.com)](https://github.com/aancw/spose)

```bash
python spose.py --proxy http://$RHOST:3128 --target $RHOST
```

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>
