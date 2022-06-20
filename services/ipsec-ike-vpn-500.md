# IPsec/IKE VPN - 500

IPSEC stands for Internet Protocol Security and is a set of technologies for safeguarding network traffic at the IP layer. There are two (2) protocols that offer varying levels of security:

1. Authentication Header (AH) - Provides data integrity to verify if the data is modified between senders, this way the data source authentication will be able to identify if the source isn’t what is expected for that connection, and protects against replay attacks.

2. Encapsulating Security Payloads (ESP) - Offers the benefit of confidentiality, which means that the data cannot be seen by anyone in the middle.

Security Associations (SA) are a collection of techniques that allow you to dynamically exchange keys and build a secure connection through AH or ESP. One of them is IKE.

## Modes

Both ESP and AH have two modes of operation:

- Transport mode - Provides security services between two hosts by applying security to the IP packet's payload, but leaving the IP headers open for routing.

- Tunneling - The entire IP packet is encrypted and/or authenticated before being sent to the other end as the payload of a new IP packet with a header. The packet is encrypted and sent based on the decrypted headers at the other end.


## VPN Client

In Linux we can use strongswan:

```sh
sudo apt install strongswan
```

# IKE Enumeration

A simple IKE scan can be as follows:

```text
ike-scan <IP>
```

Troubleshoot with strace:

```text
strace ike-scan <IP>
```

Check if there's another process using the port 500 with:

```text
lsof -i:500
```

Normally, charon could be running, kill the process:

```text
pkill charon
```

Scan IKE and save the output:

```text
ike-scan <IP> -M | tee output
```

## IKE Configuration

### VPN Client strongswan

We can install strongswan using APT:

```text
apt install strongswan
```

The configuration file are the following:

```text
cat /etc/ipsec.secrets
cat /etc/ipsec.conf
```

The documentation can be found [here](https://docs.strongswan.org/docs/5.9/config/strongswanConf.html).

### Example Configuration

Please read this [post](https://blog.ruanbekker.com/blog/2018/02/11/setup-a-site-to-site-ipsec-vpn-with-strongswan-and-preshared-key-authentication/).

The ipsec.conf file may look like the following in order to connect to IPsec:

```text
conn wixnic
    ike=3des-sha1-mod1024
    esp=3des-sha1
    type=transport
    auto=add # ondemand restart
    authby=secret
    keyexchange=ikev1
    left=YOUR_IP
    right=TARGET_IP
    rightsubment=TARGET_IP[tcp]
```

Then restart IPsec to apply the changes with:

```text
ipsec restart
```

Next, connect to the target:

```text
ipsec up wixnic
```


# References/Resources

[0xdf HTB Conceal Writeup](https://0xdf.gitlab.io/2019/05/18/htb-conceal.html#cnnecting-to-ipsec-vpn)
