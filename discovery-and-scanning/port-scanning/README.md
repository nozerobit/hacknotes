# Port Scanning

## Port Scanning Methodology

Here is my simple but effective port scanning methodology.

### Proxy

If you are scanning through a proxy you must use -Pn and -sT flags since the TCP Handshake is not received by us.&#x20;

`-sT` will do a full TCP connect scan, rather than the default `-sS` SYN scan. An SYN scan won’t work because the proxy won't be passing the TCP handshake packets back to us, so an SYN scan, which sends the SYN packet, sees the ACK, and then ends the connection, won’t be passed back over the proxy. `-Pn` also necessary because the typical host detection `nmap` does involves sending ICMP and TCP on 80 and 443. ICMP won’t go over the proxy, and 80 and 443 are likely, not going to be open, so it just returns that the host is down. `-Pn` tells `nmap` to continue scanning without that check.

> The information above was extracted from [here](https://0xdf.gitlab.io/2021/06/19/htb-tentacle.html#recon).

TL;DR: proxy support is limited right now but there are also theoretical limits of what you could do when using a proxy.

nmap can do only CONNECT and SOCKS4 and these protocols can do only TCP. Apart from that using any kind of proxy means that nmap communicates with the IP stack of the proxy and not of the target. This means:

* ICMP ping can not be done to see if a host is alive, since ICMP is not TCP. So you might need to skip the host discovery step if your targets are only accessible through the proxy (`-Pn`). Since (the unsupported) SOCKS5 ICMP does not support ICMP either this will not change in the future.
* Service discovery can be done for TCP based services only. With SOCKS5 support this could be extended to UDP.
* OS fingerprinting based on features of the IP stack is not possible because nmap does not speak with the targets IP stack when using a proxy, but instead with the proxies IP stack. This is a theoretical limit you have with any kind of proxy protocol.

Extracted from here:

{% embed url="https://security.stackexchange.com/questions/120708/nmap-through-proxy" %}

### ICMP

#### Windows

ICMP may not always be enabled in Windows, in such case you will need to use `-Pn` in your nmap scans.

TTL -> 127/8

```
ping -n 1 <IP>
```

#### Nix

ICMP is enabled by default in Unix/Linux systems, in the case that is disabled, you will need to use `-Pn` in your nmap scans.

TTL -> 64

```
ping -c 1 <IP>
```

### ICMP over IPv6

**Windows**

```
ping -6 -n 1 dead:beef::0250:56ff:feb9:dbf3
```

**Nix**

```
ping -6 -c 1 dead:beef::0250:56ff:feb9:dbf3
```

### Nmap Recon Methodology

After confirming that ICMP is enabled (if disabled use `-Pn`), I like to start up with an SYN Stealth scan on all ports scan with a high packet rate (the `--min-rate` argument can be high for a testing environment, **not** for a production environment). If you want to know why a port is open or closed, you can use the `--reason` option:

```
nmap -sS -vvv -p- --open --min-rate 5000 -n -Pn -oG scans/nmap-alltcp 10.10.10.10
```

Then, I extract the open ports and scan those ports:

```
nmap -sC -sV -n -Pn -p 22,135,139,445 -oA scans/nmap-tcpscripts 10.10.10.10
```

Second, I start a UDP scan on the top 20 most common ports:

```
nmap -sU -vvv --top-ports 20 -oG scans/nmap-udp-top20 10.10.10.10
nmap -sU -vvv -p- --min-rate 5000 --max-retries 1 -oG scans/nmap-alludp 10.10.10.74
nmap -sU -vvv -p- --max-retries 1 -oG scans/nmap-alludp-slow 10.10.10.74
```

### Nmap IPv6

Use `-6` to scan an IPv6 address:

```
nmap -sS -p- --open --min-rate 5000 -vvv -n -Pn -6 dead:beef::0250:56ff:feb9:dbf3
```

### TCP Scanning

This is a good all-purpose initial scan. Scans the most common 1000 ports with service information (-sV), default scripts (-sC), and OS detection with (-O).

> Note: Verbose mode (-v) not only provides the estimated time for each host, but it also prints the results as it goes letting you continue with the reconnaissance while scanning.

```
sudo nmap -v -sV -sC -O -T4 -n -Pn -oA nmap_scan 10.10.10.10
```

Similar scan but scans all ports, from 1 through 65535.

```
sudo nmap -v -sV -sC -O -T4 -n -Pn -p- -oA nmap_fullscan 10.10.10.10
```

### UDP Scanning

During a UDP scan (-Su) -sV will send protocol-specific probes, also known as nmap-service-probes, to every open|filtered port. In case of response the state change to open.&#x20;

```
sudo nmap -sU -sV --version-intensity 0 -n 10.10.10.10
sudo nmap -sU -p- --min-rate 5000 --max-retries 1 10.10.10.74
sudo nmap -p- -sU --max-retries 1 --open -oG nmap/UDP-scan
sudo nmap -sU --max-retries 1 --open -oG nmap/UDP-scan
```

Since UDP scanning is unreliable is a good idea to add `--max-retries` to avoid false negatives.

### Netcat Verify TCP/UDP Ports

We can verify if a **TCP** port is really open with nc:

```
nc -nvv -w 1 -z IP 3389-3390

-nvv = connect and verbose
-w = timout in seconds
-z = zero I/O mode (send no data, it is used for scanning)
```

We can verify if a **UDP** port is really open with nc:

```
nc -nv -u -w 1 -z IP 3389-3390

-nv = connect and verbose 
-u = UDP 
-w = timeout in seconds
-z = zero I/O modes (send no data, it is used for scanning)
```

### Nmap Extract Ports

Alternatively, we can extract ports like this:

```
cat filename.nmap | grep open | awk -F/ '{print $1}' ORS=','; echo
nmap -p 80,443,3389 -sC -sV 10.10.10.111
```

### Nmap Scripts

List all Nmap scripts categories:

```
grep -r categories /usr/share/nmap/scripts/.nse | grep -oP '".?"' | sort -u
```

List scripts under the **default** category:

```
grep -r categories /usr/share/nmap/scripts/*.nse | grep default | cut -d: -f1
```

### Nmap Vulnerability Scan

Vulnerability scan with **vuln** scripts:

```
nmap -sV --script vuln -oA vuln-scan 10.10.10.10
```

## Auto Reconnaissance Tools

### AutoRecon

One of the good tools out there for automatic recon is AutoRecon:

{% embed url="https://github.com/Tib3rius/AutoRecon" %}

### reconnoitre

```
reconnoitre -t ip -o output_directorry --services
reconnoitre -t ip --services --quick -o output_directory
reconnoitre -t ip -o output_directory
```

### onetwopunch

```
./onetwopunch.sh -t targets -p all -n "-sV -O --version-intensity=9"
```

### nmapAutomator

[https://github.com/21y4d/nmapAutomator](https://github.com/21y4d/nmapAutomator)

```
./nmapAutomator.sh <TARGET-IP> <TYPE>
./nmapAutomator.sh 10.1.1.1 All 
./nmapAutomator.sh 10.1.1.1 Basic  
./nmapAutomator.sh 10.1.1.1 Recon
```

If you want to use it anywhere on the system, create a shortcut using:

```
ln -s /PATH-TO-FOLDER/nmapAutomator.sh /usr/local/bin/
```

## Port Knocking

### Install Port Knock

1. `apt-get install knockd`
2. Then you simply type: `knock [ip] [port]`. For example: `knock ip 4000 5000 6000`
3. After that, you have to scan the network to see if any new port is open.
4. If you know what port is open you can connect to the port using Netcat. The following command would work `nc 192.168.1.102 8888`. This would then connect to the port.

### Nmap/Bash Open Port

```bash
for x in 4000 5000 6000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x server_ip_address; done
```



