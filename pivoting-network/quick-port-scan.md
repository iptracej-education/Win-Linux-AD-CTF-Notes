# Quick port-scan

```bash
# Run this command at the target box and update host IP address for your next target.

#!/bin/bash
host=10.5.5.11
for port in {1..65535}; do
	timeout .1 bash -c "echo >/dev/tcp/$host/$port" &&
	echo "port $port is open"
done
echo "Done"
```
