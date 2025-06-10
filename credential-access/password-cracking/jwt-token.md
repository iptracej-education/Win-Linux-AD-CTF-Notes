# JWT Token



{% code overflow="wrap" %}
```bash
# https://github.com/Sjord/jwtcrack

git clone https://github.com/Sjord/jwtcrack.git
cd jwtcrack

python crackjwt.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJkYXRhIjoie1widXNlcm5hbWVcIjpcImFkbWluXCIsXCJyb2xlXCI6XCJhZG1pblwifSJ9.8R-KVuXe66y_DXVOVgrEqZEoadjBnpZMNbLGhM8YdAc /usr/share/wordlists/rockyou.txt

# For John
jwt2john.py JWT > token 
john token --wordlist=/usr/share/wordlists/rockyou.txt
```
{% endcode %}
