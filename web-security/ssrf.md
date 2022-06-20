# Server Side Request Forgery \(SSRF\)

## SSRF Information

Server-side request forgery \(also known as SSRF\) is a web security flaw that allows an attacker to force a server-side application to send HTTP requests to any domain the attacker chooses.

The attacker may cause the server to connect to internal-only services within the organization's architecture in a conventional SSRF attack. They may also be able to force the server to connect to arbitrary external systems, exposing sensitive data such as authorization credentials.

## Enumerating SSRF

Because the application's usual traffic includes request parameters containing entire URLs, many server-side request forgery vulnerabilities are reasonably straightforward to discover. Other SSRF instances are more difficult to come by.

### Partial URLs in requests

Sometimes, an application places only a hostname or part of a URL path into request parameters. The value submitted is then incorporated server-side into a full URL that is requested. If the value is readily recognized as a hostname or URL path, then the potential attack surface might be obvious. However, exploitability as full SSRF might be limited since you do not control the entire URL that gets requested.

### URLs within data formats

Some apps send data in formats that allow URLs to be included, which may be required by the data parser for that format. The XML data format, which has been widely used in online applications to transport structured data from the client to the server, is an obvious example of this. When an application accepts data in XML format and parses it, it could be vulnerable to XXE injection and, as a result, SSRF via XXE.

### SSRF via the Referer header

Some applications employ server-side analytics software that tracks visitors. This software often logs the Referer header in requests, since this is of particular interest for tracking incoming links. Often the analytics software will actually visit any third-party URL that appears in the Referer header. This is typically done to analyze the contents of referring sites, including the anchor text that is used in the incoming links. As a result, the Referer header often represents fruitful attack surface for SSRF vulnerabilities.

## SSRF

### Bypass SSRF Defenses

### SSRF with whitelist-based input filters

## References

{% embed url="https://portswigger.net/web-security/ssrf" %}





