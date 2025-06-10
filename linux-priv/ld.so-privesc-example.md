# ld.so PrivEsc Example

{% embed url="https://book.hacktricks.xyz/linux-hardening/privilege-escalation/ld.so.conf-example" %}

### **Key Assumptions:**&#x20;

1. **Someone has created a vulnerable entry** inside a file in _/etc/ld.so.conf/, and &#x74;_&#x68;e vulnerable folder is _/home/ubuntu/lib_ (where we have writable access)

```bash
sudo echo "/home/ubuntu/lib" > /etc/ld.so.conf.d/privesc.conf
```

2. We can wait for a **reboot** or for the root user to execute **`ldconfig`**  ike cron (_in case you can execute this binary as **sudo** or it has the **suid bit** you will be able to execute it yourself_) through.&#x20;
3. Even you do not find any info under /etc/ld.so.conf or /etc/ld.so/conf.d/, run 'strace' to see if you have any missing shared library. You might find some missing so library.&#x20;

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

&#x20;      Use an exploit.c in LD\_PRELOAD section in Sudo, complie it with the same command with the \
&#x20;      filename above, and run the program. The program automatically loads the library and execute   \
&#x20;      the library.&#x20;
