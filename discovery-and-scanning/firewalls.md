# Firewalls

## Unix/Linux

### iptables

View statistics of your iptables configuration:

```text
sudo iptables -vn -L

-v = verbose
-n = numeric output
-L = list rules present in chains
```

Modify or add a firewall rule:

```text
sudo iptables -I INPUT 1 -s 10.11.1.220 -j ACCEPT
sudo iptables -I OUTPUT 1 -d 10.11.1.220 -j ACCEPT
sudo iptables -Z
```

Flags and parameters:

* INPUT = inbound chain 
* OUTPUT = outbound chain 
* 1 = rule number 
* -s = source IP
* -d = destination IP
*  -j ACCEPT = allow traffic to pass 
* -Z = zero packet and byte counters in all chains

### ufw

Firewall status \(active\|inactive\):

```text
ufw status
```

Enable/Disable firewall:

```text
sudo ufw disable
sudo ufw enable
```

Firewall block denies from an IP address:

```text
sudo ufw deny from 203.0.113.100
```

Firewall block denies from a Subnet:

```text
sudo ufw deny from 203.0.113.0/24
```

Firewall block rule for a specific interface:

```text
sudo ufw deny in on eth0 from 203.0.113.100
```

Firewall rule that allows an IP address:

```text
sudo ufw allow from 203.0.113.101
```

Firewall rule that allows traffic from a specific interface:

```text
sudo ufw allow in on eth0 from 203.0.113.102
```

Firewall rules configuration:

```text
ufw status verbose
```

List firewall rules:

```text
ufw status numbered
```

Delete/remove a firewall with its number

```text
ufw delete NUM
```

## Windows

### Windows Defender Firewall

Windows Defender Firewall with Advanced Security

![Windows Defender Firewall](../.gitbook/assets/image%20%2821%29.png)

Windows Defender Firewall -&gt; Customise Settings

![Windows Defender Firewall - Customise Settings](../.gitbook/assets/image%20%2820%29.png)

### Netsh

Before using `netsh` you should consider the following requirements

* You must have a SYSTEM-level shell \(no UAC is on SYSTEM\)
* The IP Helper service must be running and IPv6 support must be enabled on the interface we want to use.  \(Enabled by Default\) 

 Allow traffic rule:

```text
netsh advfirewall firewall add rule name="foward_port_rule" protocol=TCP dir=in localip=target_ip localport=4455 action=allow
netsh advfirewall firewall add rule name="foward_port_rule" protocol=TCP dir=in localip=192.168.134.10 localport=4455 action=allow
```

## Network

### FortiGate - UTM

Visit the FortiGate documentation for detailed information:

{% embed url="https://docs.fortinet.com/" %}



