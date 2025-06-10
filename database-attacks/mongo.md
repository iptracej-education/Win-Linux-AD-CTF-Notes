# Mongo

### Enumeration

```bash
# nmap scan
nmap -sV --script "mongo* and default" -p 27017 192.168.82.69

# check login credential
nmap -n -sV --script mongodb-brute -p 27017 192.168.82.69
```

### Connection

```bash
# https://hevodata.com/learn/mongodb-compass-ubuntu/
sudo dpkg -i mongodb-compass_1.30.1_amd64.deb 
monbodb-compass
```

[https://hevodata.com/learn/mongodb-compass-ubuntu/](https://hevodata.com/learn/mongodb-compass-ubuntu/)

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>
