# Mass Assignment

**Add a new parameter(s) and the value(s) in HTTP request, and then send it to change the behavior of the web site.**

In CTF/real world, it is common to add a parameter/value like 'is\_Admin=true' or something similar to HTTP request to pentest to become an admin for the web site. Often you will hack a source code and see the exact parameter and values to set.&#x20;

For example, in the original response, this site shows that the confirmed parameter is false.&#x20;

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

However, you want to change this parameter set to true. In your new request, add some parameter (you may need to guess) and the value to the original request, and send it.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

You could get the parameter changed to something you want to have.&#x20;

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>
