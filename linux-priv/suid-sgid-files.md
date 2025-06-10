# SUID / SGID Files

SUID files get executed with the privileges of the file owner.  SGID files get executed with the privileges of the file group. If the file is owned by root, it gets executed with root privileges, and we may be able to use it to escalate privileges.

```bash
# SUID and SGID
# -perm -g=s: Check SGID
# -perm -4000: Check SUID
# ! -type l: Not symbolic link
# -maxdepth 3: Limits the search to a maximum depth of 3 directory levels
# -exec ls -ld {} \; : Executes the ls -ld for each one of them matched
find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \; 2>/dev/null
```
