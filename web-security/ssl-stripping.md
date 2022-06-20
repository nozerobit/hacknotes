# SSL Stripping

## **SSL Stripping Information**

SSL stripping is **a technique by which a website is downgraded from https to http**. In other words, the attack is used to circumvent the security which is enforced by SSL certificates on https sites. This is also known as SSL downgrading. 

{% embed url="https://www.https.in/ssl-security/how-ssl-strip-work/" %}

## Defense

### HSTS

[HSTS](https://https.cio.gov/hsts/) \(HTTP Strict Transfer Security\) is a protocol that helps mitigate SSLstrip attacks. Each time a user establishes an HTTPS connection to a site, the site sends back a header message that says "From now on \[usually for two years\], only connect to this site over HTTPS". That information is saved by the user's browser, and if in the future the browser sees that there is a request over HTTP, it will attempt to switch to HTTPS/or it won't connect. It is important to note that not all websites that support HTTPS include HSTS response headers. Extracted from [here](https://witestlab.poly.edu/blog/ssl-stripping-attack/).

## References

{% embed url="https://doubleoctopus.com/security-wiki/threats-and-tools/ssl-stripping/" %}



