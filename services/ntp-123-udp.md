# NTP - 123/UDP

## NTP Information <a id="basic-information"></a>

The Network Time Protocol \(**NTP**\) is a networking protocol for clock synchronization between computer systems over packet-switched, variable-latency data networks.

**Default port:** 123/udp

```text
PORT    STATE SERVICE REASON123/udp open  ntp     udp-response
```

## Enumeration <a id="enumeration"></a>

```text
ntpq -c readlist <IP_ADDRESS>
ntpq -c readvar <IP_ADDRESS>
ntpq -c peers <IP_ADDRESS>
ntpq -c associations <IP_ADDRESS>
ntpdc -c monlist <IP_ADDRESS>
ntpdc -c listpeers <IP_ADDRESS>
ntpdc -c sysinfo <IP_ADDRESS>
```

```bash
nmap -sU -sV --script "ntp* and (discovery or vuln) and not (dos or brute)" -p 123 <IP>
```

## Synchronization

To make sure our local time is synchronized with the time of the KDC, we can run ntpdate:

```bash
ntpdate 10.10.10.224
```

## Examine configuration files <a id="examine-configuration-files"></a>

* ntp.conf

