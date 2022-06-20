# Ping Sweeps

Windows ping sweep one-liners:

```powershell
for /L %i in (1,1,255) do @ping -n 1 -w 200 10.10.10.%i > nul && echo 10.10.10.%i is up.
for /L %i in (1,1,255) do @ping -n 1 -w 200 172.21.10.%i > nul && echo 192.168.1.%i is up.
```

Linux ping sweep script:

```bash
#!/bin/bash

for i in {1..254}
do
    ip="10.10.10.$i"
    ping -c 1 $ip >/dev/null 2>&1 && echo -e "$ip is\e[32m UP \e[0m" || echo -e "$ip is\e[31m unreachable \e[0m" 
done
```

Alternatively, we can use this one liner:

```bash
for i in {1..254} ;do (ping -c 1 172.21.10.$i | grep "bytes from" &) ;done
```

