# Squid Proxy - 3128

## Squid Information <a id="basic-information"></a>

**Squid** is a caching and forwarding HTTP web proxy. It has a wide variety of uses, including speeding up a web server by caching repeated requests, caching web, DNS and other computer network lookups for a group of people sharing network resources, and aiding security by filtering traffic. Although primarily used for HTTP and FTP, Squid includes limited support for several other protocols including Internet Gopher, SSL, TLS and HTTPS. Squid does not support the SOCKS protocol, unlike Privoxy, with which Squid can be used in order to provide SOCKS support. Extracted from [here](https://en.wikipedia.org/wiki/Squid_%28software%29).

**Default port:** 3128

```text
PORT     STATE  SERVICE      VERSION
3128/tcp open   http-proxy   Squid http proxy 4.11
```

## Enumeration <a id="enumeration"></a>

### Web Proxy <a id="web-proxy"></a>

You can try to set this discovered service as a proxy in your browser. However, if it's configured with HTTP authentication you will be prompted for credentials.

### Nmap with Proxy <a id="nmap-proxified"></a>

Configure proxychains to use the squid proxy adding the following line at the end of the `proxychains.conf` file: `http 10.10.10.10 3128`

Then run nmap with proxychains to **scan the host from local**: 

```text
proxychains nmap -sT -n -Pn -p- localhost
```

### Curl

Using curl with proxy:

```bash
curl --proxy http://10.10.10.224:3128 http://wpad.realcorp.htb/wpad.dat
```

Use source addresses that are in an ACL, to do this configure `/etc/proxychains.conf` file:

```bash
http 10.10.10.224 3128
http 127.0.0.1 3128
http 10.197.243.77 3128
```

```bash
proxychains curl http://domain.htb/
```

