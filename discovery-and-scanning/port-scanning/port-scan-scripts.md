# Port Scan Scripts

## Bash

```bash
#!/bin/bash
for port in {1..65535}; do
    timeout .1 bash -c "echo >/dev/tcp/<IP>/$port" &&
    echo "port $port is open"
done
echo "Scan complete!"
```
