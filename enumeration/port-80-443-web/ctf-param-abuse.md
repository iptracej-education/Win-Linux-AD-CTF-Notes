# CTF - param abuse

There are some cases that you can modify the HTTP request parameter to become an admin or privileged users for web access.&#x20;

This example shows you can change the POST request parameter in body - acctype=1 to 2 to become an administrator (Step 2).&#x20;

<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

This is another example by adding the 'role' parameter and 'admin' value.  /\*\*/ is a single space in the URL to bypass a filter. Note that this can be analyzed by having the source code ready.&#x20;

<figure><img src="../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>
