# File Extension Discovery for upload

1. Open up BURP and capture the upload site.&#x20;
2. Save it to a file.&#x20;
3. Modify the saved file.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

1. Run ffuf.&#x20;

{% code overflow="wrap" %}
```bash
# options:
# -request: A saved file from BURP
# -request-proto:  Protocol to use along with raw request (default: https)
# -w: word file
# -fs: Filter the HTTP response 'size', comma separated. 
ffuf -request BURP/fuzz.req -request-proto http -w /opt/fuzz/filename_extention_list_no_dot.txt -fs 42,41

```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
