# Web Enumeration

Think of all the things that make up the web application or website, including the service that hosting it.

We need information about everything that makes up the application such as the following:

- Operating System
- Certificates
- Domains and Virtual Hosting
- Usernames
- Databases
- HTTP Headers
- HTTP Response Headers
- Web Application Version
- Language or Framework (sometimes including the **version**)
	- Some versions have vulnerabilities
- Web Application Firewalls (WAFs)
- Filters
- Forms
- Comments
- Forums
- Directories
- Files
- Configuration Files
- Language/Code
	- HTML
	- CSS
	- JavaScript
	- Razor
	- JSON
	- Libraries
		- Some version have vulnerabilities
- APIs
- Admin Panels
- Login Forms
- Forgot Password Forms
- Create/Register Users Forms
- Web Root Information (web root = where the application files and directories are located in the system)
- Others

Once we have gathered all the information we could then **what can we do with this information**?

- Is a version vulnerable?
- Look for exploits?
- Fuzz parameters?
- Fuzz the API?
- Fuzz any inputs?
- Create a wordlists based on the usernames found?
- Create a wordlists based on the keywords found?
- Attempt to SQL Injections?
- Attempt to other web vulnerabilities manually?

Tip: We can Google search things such as the following:

- Where is the login form in application x
- Where is the admin panel of application x
- Default Credentials of application x
- Pentesting application x
- Security Bug application x
- Application x version x RCE vulnerability
- Application x version x RCE exploit
- Application x github
- Application x github issue
- Application x year 

Search as many things as needed.

How can we manipulate the application?

- Inputs?
- Parameters Values?
- HTTP Headers?
- Comments?
- Forms?
- Other?

It all comes down to manipulating the website and making it do things is not intended to do or simple things such as information leakage.

## Inspecting Operating System

## Inspecting Certificate&#x20;

We can attempt to gather the certificate information with openssl:

```sh
root@kali# echo | openssl s_client -showcerts -servername 10.10.10.124 -connect 10.10.10.124:443 2>/dev/null | openssl x509 -inform pem -noout -text
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 1 (0x1)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN = ClownWare Certificate Authority, ST = LON, C = UK, emailAddress = bozo@clownware.htb, O = ClownWare Ltd., OU = ClownWare Protection Services
        Validity
            Not Before: Nov 28 14:57:03 2018 GMT
            Not After : Nov 27 14:57:03 2023 GMT
        Subject: CN = ClownWare.htb, ST = LON, C = UK, emailAddress = bozo@clownware.htb, O = ClownWare Ltd, OU = ClownWare Protection Services
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (4096 bit)
                Modulus:
                    00:c3:52:ab:a2:b7:3b:0b:92:e8:45:84:63:37:1e:
                    2c:0e:d4:2a:92:8b:e6:74:5f:76:59:db:34:62:1b:
                    ea:56:b7:2b:ef:93:78:c2:8e:96:7b:98:8a:c2:f9:
                    c2:64:0d:88:f9:d2:81:db:47:05:f9:94:b4:53:a3:
                    4a:df:f1:a6:9a:cc:2e:a8:58:b9:87:05:02:ce:3d:
                    61:a4:fc:46:ef:79:6b:59:6e:8b:b2:12:5c:6a:6e:
                    96:72:19:10:38:f5:74:75:54:c2:30:2b:0e:87:94:
                    58:86:c9:34:52:c6:86:52:ad:5c:d2:f0:9b:c0:23:
                    a0:06:ba:d3:e8:ca:0e:ab:8b:44:16:f5:71:a7:51:
                    d7:18:d8:b4:68:8c:28:c6:34:a4:0b:63:b4:34:6d:
                    7d:b8:70:a0:4e:ad:09:5f:7b:87:3c:a7:52:6d:4c:
                    74:6a:e8:5e:d1:3c:98:c1:ed:ad:33:fb:24:6b:f5:
                    ad:c6:fe:30:c5:4b:76:94:87:5c:70:dd:d4:4c:84:
                    29:8d:23:33:ff:ee:fc:78:51:f8:88:ca:3c:f0:2b:
                    a5:f6:ff:b1:7a:69:49:40:cc:89:bb:e6:3c:43:b2:
                    39:b4:5f:58:87:be:1d:58:d9:38:fa:c4:0a:0a:1e:
                    d7:73:50:28:60:6a:09:c8:63:3b:48:e7:d3:3f:ac:
                    45:92:64:65:7f:83:11:5b:cb:df:f1:65:cd:07:d8:
                    20:39:84:a7:9d:61:12:3a:5c:75:26:57:8b:bb:02:
                    f0:61:50:67:55:b3:2c:e4:e6:b9:12:6c:f5:c5:91:
                    24:59:63:ca:2b:10:31:2a:55:3d:15:3c:4e:82:ee:
                    d3:e6:77:29:57:13:d6:04:02:ae:b1:ff:98:4a:38:
                    53:18:da:19:66:ac:17:1e:bd:8e:90:0b:d7:22:a7:
                    04:b5:69:0a:92:db:0a:56:ca:15:87:0c:ba:9e:ef:
                    19:2a:cd:0a:66:bb:8c:dc:f2:a5:f1:5e:c3:b8:18:
                    00:e4:33:ce:b9:e5:c2:00:9e:70:e6:9e:22:9d:2d:
                    37:16:66:ae:0d:64:73:11:b6:8e:28:84:d1:32:06:
                    4f:41:e9:51:7d:93:14:f1:31:53:ab:ee:c2:6b:b6:
                    0f:fc:31:2f:e2:d5:09:fe:c8:44:2b:c3:6f:e0:df:
                    df:f5:c8:b6:ef:1e:a1:81:58:ea:ca:78:ec:af:0b:
                    fc:9e:ef:95:63:ac:6b:7d:f6:81:d6:74:81:dd:e3:
                    f3:7c:ab:ed:fc:a5:15:ab:e9:98:99:7b:99:05:0f:
                    bc:4d:d8:a0:6a:a3:32:71:31:02:08:2c:be:4d:7e:
                    9c:db:53:3e:fb:05:db:4c:75:b0:0e:66:b4:8c:6a:
                    2b:30:b3
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            X509v3 Key Usage: 
                Digital Signature, Non Repudiation, Key Encipherment
            X509v3 Subject Alternative Name: 
                DNS:clownware.htb, DNS:sni147831.clownware.htb, DNS:*.clownware.htb, DNS:proxy.clownware.htb, DNS:console.flujab.htb, DNS:sys.flujab.htb, DNS:smtp.flujab.htb, DNS:vaccine4flu.htb, DNS:bestmedsupply.htb, DNS:custoomercare.megabank.htb, DNS:flowerzrus.htb, DNS:chocolateriver.htb, DNS:meetspinz.htb, DNS:rubberlove.htb, DNS:freeflujab.htb, DNS:flujab.htb
    Signature Algorithm: sha256WithRSAEncryption
         90:9b:f0:9a:be:21:1e:0b:d6:fc:d5:1d:57:b1:e0:c2:a2:77:
         8f:b0:a6:c8:5b:83:a2:2a:f5:63:cd:8e:26:53:b5:42:35:f2:
         f5:8d:57:4a:e4:91:f9:8a:92:e3:37:f2:8a:cf:08:d7:92:cb:
         d1:8d:39:7b:ca:5d:cf:b7:f8:d6:3c:34:5a:17:f3:d8:d0:f6:
         ac:07:0f:e4:d5:a6:ec:44:21:ff:cb:27:4d:8c:d0:56:85:fa:
         06:75:26:79:e5:4a:9b:1f:99:e9:6b:f1:d7:c9:17:cd:59:08:
         d1:bb:31:d3:41:f6:c6:27:22:34:eb:56:d2:1e:3b:ad:23:e0:
         ea:a0:72:56:7a:73:07:c6:03:0d:6d:50:cc:97:92:d9:01:68:
         b4:fa:f3:6b:cd:d6:f7:0e:b6:b3:97:28:db:50:10:e0:e1:df:
         61:27:58:b2:5f:39:94:8f:ec:18:f8:a1:f4:1f:e4:4c:8c:c3:
         fb:13:f9:1d:1b:e2:9a:62:3e:5b:c7:6e:1a:c2:7f:87:3c:4d:
         84:ac:03:60:50:30:3d:42:de:66:9f:3c:07:f1:35:05:62:54:
         7d:cd:9a:af:34:00:08:80:c9:ac:38:fd:86:94:51:b0:ef:77:
         66:6c:4e:08:0a:07:59:fb:06:b7:5c:46:ce:45:39:0e:d4:bd:
         c3:b8:f7:4b:5b:64:41:4e:32:0c:ff:82:68:8b:93:be:53:3f:
         cd:5a:fe:23:d2:04:61:8d:b2:7c:23:03:9c:8c:c0:07:61:36:
         9d:05:fd:b6:3d:c3:d4:33:b8:42:12:98:04:b1:ca:c7:67:4e:
         cb:a8:7a:aa:aa:b6:32:8b:8a:57:8b:92:da:ab:a5:e5:1a:4e:
         25:41:06:81:e3:d4:f7:84:9e:a3:bd:e3:09:29:4f:0a:76:17:
         b7:53:b5:a0:05:4b:5b:35:8e:68:0f:2a:93:ac:ed:27:7f:9f:
         4c:a6:bb:f7:71:15:c7:ff:63:d2:74:9d:72:95:3c:b9:0f:a6:
         86:c3:e5:95:e0:10:71:4a:3a:14:9c:f6:dd:2b:e0:b0:e5:7a:
         e4:95:01:8b:25:2f:08:75:24:51:de:7b:95:da:4e:71:f0:6d:
         1b:20:a5:ad:2a:65:b7:b3:17:43:96:04:2f:81:93:82:28:c4:
         fa:3d:83:99:d8:01:39:e7:2c:6b:11:53:f9:77:00:86:b5:aa:
         32:17:40:ea:e2:0a:81:73:08:45:42:07:4c:be:a8:72:1b:7d:
         bd:85:a1:bd:dc:6c:33:bb:11:01:df:0f:cc:a7:42:45:4b:e5:
         51:55:bb:d8:33:c1:c4:e7:e0:52:1a:61:7a:5e:98:9b:d1:9e:
         54:83:70:d1:09:7f:1d:20
```

We could also find domain names:

```sh
root@kali# echo | openssl s_client -showcerts -servername 10.10.10.124 -connect 10.10.10.124:443 2>/dev/null | openssl x509 -inform pem -noout -text | grep DNS | tr "," "\n" | cut -d: -f2
clownware.htb
sni147831.clownware.htb
*.clownware.htb
proxy.clownware.htb
console.flujab.htb
sys.flujab.htb
smtp.flujab.htb
vaccine4flu.htb
bestmedsupply.htb
custoomercare.megabank.htb
flowerzrus.htb
chocolateriver.htb
meetspinz.htb
rubberlove.htb
freeflujab.htb
flujab.htb
```

## Inspecting Virtual Hosting

A SSL/TLS certificate may reveal a virtual host in under the following:

- Subject Name
- Common Name

We can also attempt to find domain names in web pages.

We could also attempt to fuzz for virtual host domain names.

## HTTP Headers

The HTTP Headers can reveal sensitive information and/or they could be vulnerable. It is also recommended to do some research on each header including it's version.

A few headers that we can **almost** always use to enumerate the service and application are the following:

- Server: Reveals the HTTP service
- X-Powered-By: Reveals the application language or framework

We can enumerate HTTP headers with curl:

```sh
curl -I http://bighead.htb/
```

Alternatively, we can use Burp Proxy.

Alternatively, we can use the browser development console.

We could also use other tools such as whatweb.

We could just use our own scripts or tools as well.

## Enumerating Usernames

We can find usernames in this possible sections:

- APIs
- Forms
- Forums
- Comments
- Author Fields (Often seen in articles or blog posts)
- Databases
- Admin Panels
- Control Panels
- Fuzzing parameters values
- Others

### User-Agent



#### ShellShock

### Response Headers

## Parameters

### GET Parameters

### POST Parameters

## Input

### Forms

### Comments

### Text Boxes

### Search Bars

## DevTools

### Sources

### Network&#x20;

### Storage&#x20;

## Administration Consoles | Control Panels

## Inspecting Sitemaps

## Inspecting Page Content

## Inspecting URLs

## Inspecting Databases

## Inspecting Frameworks

## Inspecting Software or Applications

## Web Fuzzing | Find Directories, Files, Subdomains, Virtual Hosting, Parameters, and Values

Check out this page:

{% embed url="https://nozerobit.gitbook.io/hacknotes/web-security/web-fuzzing" %}

## Sensitive Files

Search **every** website and sub directory for interesting services or files containing potential usernames and/or passwords.

## Tools

### nikto

nikto is a good at finding stuff, it is especially useful when a sitemap is present, i.e, robots.txt:

```
nikto -h <IP>
```



