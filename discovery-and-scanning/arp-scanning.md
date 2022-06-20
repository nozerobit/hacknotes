# ARP Scanning

We can use `netdiscover` to perform an ARP scan:

```bash
netdiscover -i eth0
netdiscover -r 172.21.10.0/24
```

Alternatively, we can use `nbtscan` to do the same thing:

```bash
nbtscan -r 172.21.1.0/24
```

We could also use `nmap` as well:

```bash
nmap -sn 172.21.10.0/24
nmap -sn 172.21.10.1-253
nmap -sn 172.21.10.*
nmap -n -sn -PR --packet-trace --send-eth 192.168.33.37
```
