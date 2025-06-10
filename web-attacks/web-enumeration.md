# Web Enumeration

### Common Techniques

* Check all tool-based enumerations in Enumeration [Port 80/443 - Web](https://app.gitbook.com/o/sbYQpaBkV0ueQjvLIdTt/s/nA4bAkddGXesk1QCLYAY/~/changes/176/enumeration/port-80-443-web) section - nmap, feroxbuster, wfuzz, etc. check [`Port 80/443`](../enumeration/port-80-443-web/) Web section.&#x20;
* Find any Platform/Framework/CMS/Web Applications for vulnerability. Check version numbers - OS, Web Server & Script language, Platform, API, CMS and Components, etc. This example shows 'metabase' application  and google 'metabase RCE'.&#x20;

<figure><img src="../.gitbook/assets/image (114).png" alt="" width="359"><figcaption></figcaption></figure>

* Google the 'name' 'version' and 'exploit' to check any exploit code available.&#x20;
* Learn pages and links for attack vectors.&#x20;
* Check any usernames or email on the pages.&#x20;
* Check source codes of web pages to see any embedded or hidden information that their web developers left.&#x20;
* Check GitHub directory or gitea server running.&#x20;
* Check all input that you could potentially control. Start with SQL, LFI, SSRF, Command Injection, etc.
* For sign in, start signing in with command username and password combinations. <mark style="color:red;">Please pay special attention to the HTTP request parameters that you could manipulate or add to become an admin or high privilege user.</mark>&#x20;
* For registration, register a username and password and sign in with the user. <mark style="color:red;">Please pay special attention to the HTTP request parameters that you could manipulate or add a new parameter to become an admin or high privilege user.</mark>&#x20;
* Find any upload function.&#x20;
* Change GET to POST or POST to GET requests to see if anything happens.
* Find any URL that you could fuzz further. This example shows the number (100) as a part of URL for content detail. You could fuzz this web site by fuzzing with different numbers.&#x20;

<figure><img src="../.gitbook/assets/image (115).png" alt="" width="375"><figcaption></figcaption></figure>

* Find an obvious function to investigate further. For example, the page below shows 'Submit a repo' that will compile your source code to run. This looks obvious for a RCE opportunity!&#x20;

<figure><img src="../.gitbook/assets/image (113).png" alt="" width="375"><figcaption></figcaption></figure>

* Enumerate continuously via Json response. The 'jq' command is your friend. The following example shows that we get some hash value for a username. In this case, it was a cookie value to the user to sign in the web site.&#x20;

<figure><img src="../.gitbook/assets/image (117).png" alt="" width="350"><figcaption></figcaption></figure>
