# Passive Information Gathering

## OSINT Framework

Look up information from almost everywhere:

{% embed url="https://osintframework.com/" %}

## AI Facial Recognition

The internet is scary... you can find a history about a person if there are traces of him/her on the internet.&#x20;

Here is a list of websites or search engines that you can use to perform AI Facial Recognition:

{% embed url="https://pimeyes.com/en" %}

{% embed url="https://images.google.com" %}

{% embed url="http://www.pictriev.com" %}

{% embed url="https://tineye.com" %}

## Emails Searching

You can search for emails with this website:

{% embed url="https://hunter.io" %}

## Whois Enumeration

Whois is a TCP service focused tool that can be used to gather information about a domain name. This tool uses a database to provide this type of information.&#x20;

We can find basic information about a domain name with forward search:

```
whois <domain_here.com>
```

## Maltego

Collect information and create diagrams as you go:

{% embed url="https://www.maltego.com/" %}

## Shodan

Shodan is the world's first search engine for Internet-connected devices and the World Wide Web. Discover how Internet intelligence can help you make better decisions.

{% embed url="https://www.shodan.io" %}

You can find:

* Websites
* Routers
* IoT Devices
* Cameras&#x20;
* Ports&#x20;
* Services
* Vulnerabilities&#x20;
* Web Technologies&#x20;
* Public IPs and much more...

## Stack Overflow

Stack Overflow is a website for developers to ask and answer questions related to coding/programming.

If we find information about an employer and we also found his/her account in stack overflow, we can see if which programming language the organization uses by the questions that the he/she ask or answers.

{% embed url="https://stackoverflow.com" %}

## theHarverster

Gather information from a public domain using all available search engines:

```
theHarvester -d domain.com -b all
```

Gather information from a public domain using multiple search engines:

```
theHarvester -d domain.com -b google,trello,bing,dogpile
```

Gather information from a public domain using google:

```
theHarvester -d domain.com -b google
```

## GitHub Repositories

We can search for possible GitHub repositories of the target:

```
<service> site:github.com
```

### GitHub/GitLab Leaks

{% embed url="https://github.com/michenriksen/gitrob" %}

{% embed url="https://github.com/zricethezav/gitleaks" %}

## Site Information

### Netcraft

{% embed url="https://www.netcraft.com/" %}

Google Chrome Extension

{% embed url="https://chrome.google.com/webstore/detail/netcraft-extension/bmejphbfclcpmpohkggcjeibfilpamia" %}

Firefox Extension

{% embed url="https://addons.mozilla.org/en-GB/firefox/addon/netcraft-toolbar/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search" %}

### Security Headers

Analyze HTTP Headers:

{% embed url="https://securityheaders.com/" %}

### SSL/TLS

Analyze SSL/TLS settings:

{% embed url="https://www.ssllabs.com/ssltest/" %}

### Recon-ng

Recon-ng is a full-featured Web Reconnaissance framework written in Python. Complete with independent modules, database interaction, built in convenience functions, interactive help, and command completion, Recon-ng provides a powerful environment in which open source web-based reconnaissance can be conducted quickly and thoroughly.

Recon-ng has a look and feel similar to the Metasploit Framework, reducing the learning curve for leveraging the framework. However, it is quite different. Recon-ng is not intended to compete with existing frameworks, as it is designed exclusively for web-based open source reconnaissance. If you want to exploit, use the Metasploit Framework. If you want to Social Engineer, use the Social Engineer Toolkit.

{% embed url="https://www.kali.org/tools/recon-ng" %}

## Site Specific Tools

### Social Media

Search for information from social media sites:

{% embed url="https://www.social-searcher.com/" %}

Twitter Information:

{% embed url="https://digi.ninja/projects/twofi.php" %}

LinkedIn Information:

{% embed url="https://github.com/initstring/linkedin2username" %}

## Code Leaks

### Pastebin

Some people share sensitive codes for the public:

```
<topic> site:pastebin.com
```

## Tools Packages

Kali Linux has a good package to install a-lot of tools:

{% embed url="https://www.kali.org/tools/kali-meta#kali-tools-information-gathering" %}
