# Web Fingerprinting

## Identify Web Technologies

### Headers

**Server:** (IIS,Apache,Nginx,Others)&#x20;

**X-Powered-By:** (PHP,ASP.NET,JSP,JBoss,Others)&#x20;

**Cookies:** (PHPSESSID=XXXX, ASPSESSIONIDYYYY=XXXX, JSESSION=XXXX)

Enumerate headers with curl:

```
curl -I <IP>
```

Enumerate headers with nc:

```
nc <IP> <PORT>

> Once we establish the connection with netcat

Type HEAD / HTTP/1.0 and hit enter two times
```

### Whatweb

Whatweb is a really good tool for web enumeration, we can use -v for a more readable output:

```
whatweb -v <IP>
```

### Wappalyzer

Wapplyzer is a good browser extension for identifying web technologies:

{% embed url="https://addons.mozilla.org/en-GB/firefox/addon/wappalyzer/" %}

