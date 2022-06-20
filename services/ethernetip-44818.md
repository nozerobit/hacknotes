# EthernetIP - 44818

TCP/44818 is queried with a list of Identities Messages to positively identify an EtherNet/IP device \(0x63\). The response messages will identify if the device is an EtherNet/IP device and then parse the data to enumerate it.

**Default port:** 44818 UDP/TCP

```text
PORT STATE SERVICE
44818/tcp open EtherNet/IP
```

## Enumeration

```text
nmap -n -sV --script enip-info -p 44818 <IP>
pip3 install cpppo
python3 -m cpppo.server.enip.list_services [--udp] [--broadcast] --list-identity -a <IP>
```

##  <a id="shodan"></a>

