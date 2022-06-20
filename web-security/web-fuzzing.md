# Web Fuzzing

Tip: Don't stick to one (1) tool, try to use multiple ones.

## Directory Fuzzing

We should be able to use ffuf to locate website directories now that we grasp the notion of Web Fuzzing and know our wordlist.

### Directory Fuzzing

To refer to a wordlist where we wish to fuzz, we can give it a keyword. For instance, we can take our wordlist and add the keyword FUZZ to it by appending `:FUZZ` to the end of it.

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ
```

Next, because we want to fuzz for web directories, we may use the FUZZ term in our URL where the directory would be:

```
wixnic@htb[/htb]$ ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ
```

Let's now begin our objective in the question below and execute our last command.

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : http://SERVER_IP:PORT/FUZZ
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

<SNIP>
dir                    [Status: 301, Size: 326, Words: 20, Lines: 10]
:: Progress: [87651/87651] :: Job [1/1] :: 9739 req/sec :: Duration: [0:00:09] :: Errors: 0 ::
```

_This speed may vary depending on your internet speed and ping if you used `ffuf` on your machine, but it should still be extremely fast._

If we are in a rush, we can increase the number of threads to 200, for example, with `-t 200`, but this is not recommended, especially when used on a remote site, as it may cause a **Denial of Service**, or even bring your internet connection down in severe circumstances!

## Page Fuzzing

Through the usage of wordlists and keywords, we now have a fundamental understanding of how to use **ffuf**. After that, we'll look at how to find pages.

### Extension Fuzzing

We discovered that we had access to `/dir`in the previous phase, but the directory returned an empty page, and we were unable to manually locate any links or pages. As a result, we'll use web fuzzing once more to see if the directory contains any hidden pages.

However, before we begin, we must determine whether the website uses `.html,.aspx,.php`, or another sort of page.

Finding the server type from the HTTP response headers and assuming the extension is a standard approach to do it. For example, if the server is Apache, the extension might be.php; if the server is IIS, the extension might be `.asp` or `.aspx`; and so on.

However, this strategy is not particularly practical. So, similarly to how we fuzzed directories, we'll use ffuf to fuzz the extension. We would put the FUZZ keyword where the extension would be instead of where the directory name would be. FUZZ, and for popular extensions, utilize a wordlist.

For extensions, we can use the following wordlist in SecLists:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ <SNIP>
```

We must first indicate which file that extension will be at the end of before we begin fuzzing!

We can always use two wordlists, each with its own unique keyword, and then fuzz both with `FUZZEXT`.

The `index.*` is a file that we can always locate on most websites, so we'll use it as our file and add fuzz extensions to it.

Now, we can rerun our command, carefully placing our `FUZZ` keyword where the extension would be after `index`:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/dir/indexFUZZ

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : http://SERVER_IP:PORT/dir/indexFUZZ
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 5
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

.php                    [Status: 200, Size: 0, Words: 1, Lines: 1]
.phps                   [Status: 403, Size: 283, Words: 20, Lines: 10]
:: Progress: [39/39] :: Job [1/1] :: 0 req/sec :: Duration: [0:00:00] :: Errors: 0 ::
```

We do get a couple of hits, but only `.php` gives us a response with code `200`. Great! We now know that this website runs on `PHP` to start fuzzing for `PHP` files!

### Page Fuzzing

We will now use the same concept of keywords we've been using with `ffuf`, use `.php` as the extension, place our `FUZZ` keyword where the filename should be, and use the same wordlist we used for fuzzing directories:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/dir/FUZZ.php

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : http://SERVER_IP:PORT/dir/FUZZ.php
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

index                   [Status: 200, Size: 0, Words: 1, Lines: 1]
home                   [Status: 200, Size: 465, Words: 42, Lines: 15]
:: Progress: [87651/87651] :: Job [1/1] :: 5843 req/sec :: Duration: [0:00:15] :: Errors: 0 ::
```

We get a couple of hits; both have an HTTP code 200, meaning we can access them. index.php has a size of 0, indicating that it is an empty page, while the other does not, which means that it has content.

## Recursive Fuzzing

So far, we've fuzzed for directories, then gone further into these directories, and finally fuzzed for files. However, if we had dozens of folders, each with its own subdirectories and files, it would take a long time to finish.

To be able to automate this, we will utilize what is known as `recursive fuzzing`.

### Recursive Flags

When we scan recursively, it automatically starts another scan under any newly identified directories that may have on their pages until it has fuzzed the main website and all of its subdirectories.

Some websites may contain a large tree of sub-directories, such as /login/user/content/uploads/...etc, which may grow the scanning tree and make scanning them all take a long time. To prevent this, we must give a depth for our recursive scan, which will prevent it from scanning directories further than that depth. We may then pick the most intriguing direct after we've fuzzed the initial directories.

With the -recursion flag in **ffuf**, we may enable recursive scanning and specify the depth with the -recursion-depth parameter. Only the main directories and their direct sub-directories will be fuzzed if we use `-recursion-depth 1`. It will not fuzz any sub-sub-directories discovered, such as `/login/user`, for pages.

We can define our extension with **-e **_**\*\*\*\***_** .php** while utilizing recursion in ffuf.

_Note: we can still use `.php` as our page extension, as these extensions are usually site-wide._

Finally, we will also add the flag `-v` to output the full URLs.

### Recursive Scanning

Let's run the first command again, this time adding the recursion flags and specifying.php as our extension, and see what happens:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : http://SERVER_IP:PORT/FUZZ
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Extensions       : .php 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

[Status: 200, Size: 986, Words: 423, Lines: 56] | URL | http://SERVER_IP:PORT/
    * FUZZ: 

[INFO] Adding a new job to the queue: http://SERVER_IP:PORT/forum/FUZZ
[Status: 200, Size: 986, Words: 423, Lines: 56] | URL | http://SERVER_IP:PORT/index.php
    * FUZZ: index.php

[Status: 301, Size: 326, Words: 20, Lines: 10] | URL | http://SERVER_IP:PORT/blog | --> | http://SERVER_IP:PORT/blog/
    * FUZZ: blog

<...SNIP...>
[Status: 200, Size: 0, Words: 1, Lines: 1] | URL | http://SERVER_IP:PORT/blog/index.php
    * FUZZ: index.php

[Status: 200, Size: 0, Words: 1, Lines: 1] | URL | http://SERVER_IP:PORT/blog/
    * FUZZ: 

<...SNIP...>
```

As we can see, the scan took far longer this time, sending about six times as many requests, and the wordlist nearly doubled in size once with `.php` and once without.

Despite this, we were able to obtain a significant number of results, including all of the ones we had previously recognized, all with a single command.

## Sub-domain Fuzzing

We'll learn how to use ffuf to find sub-domains (i.e., \*.website.com) for any website in this part.

### Sub-domains

Fortunately, there is a section in the SecLists repo dedicated to sub-domain wordlists, which contains often used words for sub-domains. It's located in the directory `/opt/SecLists/Discovery/DNS/`. In our situation, we'll use `subdomains-top1million-5000.txt`, which is a shorter wordlist. We can choose a longer list if we want to extend our scan.

In terms of our target, we'll use \<domain\_name> and execute our scan on it. Let's try ffuf with the `FUZZ` keyword instead of sub-domains to see if we get any results:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.domain.test/

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : https://FUZZ.domain.test
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200
________________________________________________

forum                   [Status: 200, Size: 72197, Words: 3664, Lines: 675]
www                     [Status: 200, Size: 21268, Words: 1720, Lines: 1]
help                    [Status: 200, Size: 25830, Words: 5049, Lines: 364]
<...SNIP...>
```

We see that we do get a few hits back. We can verify that these are actual sub-domains by visiting one of them:

We see that indeed these are working sub-domains. Now, we can try running the same thing on `subdomain.test` and see if we get any hits back:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.subdomain.test/

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : https://FUZZ.subdomain.test/
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

:: Progress: [4997/4997] :: Job [1/1] :: 131 req/sec :: Duration: [0:00:38] :: Errors: 4997 ::
```

This means that there are no `public` sub-domains under `subdomain.test`, as it does not have a public DNS record, as previously mentioned.

## Vhost Fuzzing

We were able to fuzz public sub-domains using public DNS information, as we observed in the preceding section. However, we were unable to utilize the same strategy to fuzz sub-domains that did not have a public DNS record or sub-domains beneath non-public websites. We'll learn how to achieve that with Vhost Fuzzing in this part.

### Vhosts vs. Sub-domains

The fundamental distinction between VHosts and sub-domains is that a VHost is essentially a sub-domain that is served on the same server and has the same IP, allowing a single IP to serve several websites.

`VHosts may or may not have public DNS records.`

Many websites contain non-public sub-domains that are not published in public DNS records, thus when we visit them via a browser, we will be unable to connect since the public DNS does not know their IP address. If we employ sub-domain fuzzing again, we will only be able to identify public sub-domains and will not be able to discover any non-public sub-domains.

This is where we use VHosts Fuzzing on an IP address that we already own. We'll execute a scan and check for other scans on the same IP, allowing us to detect both public and private sub-domains and VHosts.

### Vhosts Fuzzing

We will fuzz HTTP headers, specifically the `Host:` header, to scan for VHosts without manually adding the complete wordlist to our /etc/hosts. To do so, we'll define a header using the `-H` flag and utilize the FUZZ keyword within it, as shown below:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://subdomain.test:PORT/ -H 'Host: FUZZ.subdomain.test'

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : http://subdomain.test:PORT/
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Header           : Host: FUZZ
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

mail2                   [Status: 200, Size: 900, Words: 423, Lines: 56]
dns2                    [Status: 200, Size: 900, Words: 423, Lines: 56]
ns3                     [Status: 200, Size: 900, Words: 423, Lines: 56]
dns1                    [Status: 200, Size: 900, Words: 423, Lines: 56]
lists                   [Status: 200, Size: 900, Words: 423, Lines: 56]
webmail                 [Status: 200, Size: 900, Words: 423, Lines: 56]
static                  [Status: 200, Size: 900, Words: 423, Lines: 56]
web                     [Status: 200, Size: 900, Words: 423, Lines: 56]
www1                    [Status: 200, Size: 900, Words: 423, Lines: 56]
<...SNIP...>
```

We can see that all of the terms in the wordlist return 200 OK. This is to be expected, given that we are merely modifying the header when visiting [http://subdomain.test:PORT/](http://subdomain.test2/:PORT/). As a result, we know we'll always receive 200 OK. However, if the VHost exists and we send the proper one in the header, we should get a different response size because we'd be getting the page from that VHost, which is most likely to show a distinguished page.

## Filtering Results

We haven't applied any filtering to our ffuf yet, and the results are filtered by default based on their HTTP code, which filters out code 404 NOT FOUND and keeps the rest. However, as we observed in our earlier ffuf test, code 200 can yield a large number of replies. As a result, we'll need to filter the results depending on another factor, which we'll learn about in this section.

### Filtering

Ffuf allows you to match or filter out specific HTTP codes, response sizes, or word counts. With ffuf -h, we may see this:

```
wixnic@htb[/htb]$ ffuf -h
...SNIP...
MATCHER OPTIONS:
  -mc              Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403)
  -ml              Match amount of lines in response
  -mr              Match regexp
  -ms              Match HTTP response size
  -mw              Match amount of words in response

FILTER OPTIONS:
  -fc              Filter HTTP status codes from response. Comma separated list of codes and ranges
  -fl              Filter by amount of lines in response. Comma separated list of line counts and ranges
  -fr              Filter regexp
  -fs              Filter HTTP response size. Comma separated list of sizes and ranges
  -fw              Filter by amount of words in response. Comma separated list of word counts and ranges
<...SNIP...>
```

We can't utilize matching in this scenario since we don't know how big the responses from other VHosts will be. We know the response size of the wrong results, which is 900 in this case (as seen in the test above), and we can filter it out using -fs 900. Now, let's run the same command again, but this time with the above flag added, and see what we get:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://subdomain.test2:PORT/ -H 'Host: FUZZ.subdomain.test' -fs 900

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : http://subdomain.test:PORT/
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Header           : Host: FUZZ.subdomain.test2
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
 :: Filter           : Response size: 900
________________________________________________

<...SNIP...>
admin                   [Status: 200, Size: 0, Words: 1, Lines: 1]
:: Progress: [4997/4997] :: Job [1/1] :: 1249 req/sec :: Duration: [0:00:04] :: Errors: 0 ::
```

This returns the host `admin.subdomain.test` which is a valid host.

## Parameter Fuzzing

### GET Request Fuzzing

We'll use ffuf to enumerate parameters in the same way we fuzzed various areas of a website. Let's start with fuzzing GET requests, which are often given right after the URL and preceded by a `?`symbol, such as:

* `http://admin.subdomain.test:PORT/dir/filename.php?param1=key`.

So, in the case above, all we have to do is substitute `param1` with `FUZZ` and perform our scan. However, before we begin, we must first choose a suitable wordlist.

SecLists has a file called burp-parameter-names.txt in /opt/SecLists/Discovery/Web-Content/burp-parameter-names.txt that does exactly that. We can now perform our scan.

We're going to get a lot of results this time, so we'll filter out the default response size.

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.subdomain.test:PORT/dir/filename.php?FUZZ=key -fs xxx

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : GET
 :: URL              : http://admin.subdomain.test:PORT/dir/filename.php?FUZZ=key
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
 :: Filter           : Response size: xxx
________________________________________________

<...SNIP...>                    [Status: xxx, Size: xxx, Words: xxx, Lines: xxx]
```

We do get a hit back, with this information we can try to visit the page and add this `GET` parameter.

### POST Request Fuzzing

The key distinction between POST and GET requests is that POST requests do not include the URL and cannot be appended after a `?` sign.

The data field in an HTTP request is used to pass POST requests. We may use the `-d` flag with ffuf to fuzz the data field. To send POST requests, we must also include `-X POST`.

> Tip: In PHP, "POST" data "content-type" can only accept "application/x-www-form-urlencoded". So, we can set that in "ffuf" with "-H 'Content-Type: application/x-www-form-urlencoded'".

So, let us repeat what we did earlier, but place our `FUZZ` keyword after the `-d` flag:

```
wixnic@htb[/htb]$ ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.subdomain.test:PORT/dir/filename.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx

       /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : POST
 :: URL              : http://admin.subdomain.test:PORT/dir/filename.php
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Data             : FUZZ=key
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
 :: Filter           : Response size: xxx
________________________________________________

id                      [Status: xxx, Size: xxx, Words: xxx, Lines: xxx]
<...SNIP...>
```

As we can see this time, we got a couple of hits, the same one we got when fuzzing `GET` and another parameter, which is `id`.

## Value Fuzzing

Our command should be similar to the POST command we used to fuzz for parameters, with the exception that the FUZZ keyword should be placed where the parameter value would be, and we'll use a wordlist we just created:

```
wixnic@htb[/htb]$ ffuf -w wordlist.txt:FUZZ -u http://admin.subdomain.test:PORT/dir/filename.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx

       /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v1.0.2
________________________________________________

 :: Method           : POST
 :: URL              : http://admin.subdomain.test:PORT/dir/filename.php
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Data             : id=FUZZ
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
 :: Filter           : Response size: xxx
________________________________________________

<...SNIP...>                      [Status: xxx, Size: xxx, Words: xxx, Lines: xxx]
```

We see that we get a hit right away.

# Fuzzing via Proxy

Gobuster:

```bash
[HTTP_PROXY="socks5://127.0.0.1:1080/"] gobuster dir -u http://$IP -w /usr/share/dirb/wordlists/common.txt -o gobuster.txt
```

# Web Fuzzing Tools

You may want to use other tools besides fuff:

{% embed url="https://tools.kali.org/web-applications/dirb" %}

{% embed url="https://github.com/OJ/gobuster" %}

{% embed url="https://github.com/maurosoria/dirsearch" %}

{% embed url="https://wfuzz.readthedocs.io/en/latest/user/installation.html" %}

# Common Wordlists

| **Command**                                                               | **Description**         |
| ------------------------------------------------------------------------- | ----------------------- |
| `/opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt` | Directory/Page Wordlist |
| `/opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt`           | Extensions Wordlist     |
| `/opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`      | Domain Wordlist         |
| `/opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt`     | Parameters Wordlist     |

## Useful Wordlists

These are the list of wordlists that have helped me the most:

* /usr/share/seclists/Discovery/Web-Content/common.txt
* /usr/share/seclists/Discovery/Web-Content/big.txt
* /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt
* /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt 
* /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

Technology specific wordlists:

* /usr/share/seclists/Discovery/Web-Content/IIS.fuzz.txt 


# Fuzzing Cheatsheet

## nikto

Enumerate directories with nikto:

```
nikto -h <URL>
```

## feroxbuster

It seems that feroxbuster is fine as hell:

{% embed url="https://epi052.github.io/feroxbuster-docs/docs/compare" %}

Enumerate directories:

```
./feroxbuster -u <URL> -w <WORDLIST> -x php,txt,zip
```

## dirsearch

Enumerate directories:

```
python3 dirsearch.py -u <URL> -w <WORDLIST> -r -t 60 --full-url
```

## wfuzz

Common Flags:

* \-c= for colorized mode
* –hc=404 for omitting routes where the response is 404
* \-t 200= is for giving threads number
* –hw= for not taking care of word number return
* –hh=73 for not taking care of characters with 73 as return number

Directory Fuzzing:

```
wfuzz -c -t 200 --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.10.83/FUZZ
```

Files and Extensions Fuzzing

```
wfuzz -c -t 200 --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -w extensions http://10.10.10.83/FUZZ.FUZ2Z
```

Virtual Host Fuzzing (hide responses with --hw / hide words):

```
wfuzz -c -t 200 --hc=404 --hw=12 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -H "Host: FUZZ.sneakycorp.htb" http://10.10.10.197
wfuzz -c -t 200 --hc=404 --hw=28,73 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -H "Host: FUZZ.localhost.com http://loalhost.com
wfuzz -u http://object.htb -H 'Host: FUZZ.object.htb' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --hh 29932
```

GET Request Parameter Fuzzing (hide responses with --hw / hide words):

```
wfuzz -c -t 200 --hc=404 --hw=0 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://sec03.rentahacker.htb/shell.php?FUZZ=whoami
```

## gobuster

Virtual Host Fuzzing:

```
gobuster vhost -u http://sneakycorp.htb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Enumerate directories on HTTP:

```
gobuster dir -u <URL> -w <WORDLIST> -s 200 -x txt,zip,php
```

Enumerate directories on HTTPS:

```
gobuster dir -u <URL> -w <WORDLIST> -s 200 -x txt,zip,php -k
```

## ffuf

Directory Fuzzing

```bash
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```

Extension Fuzzing

```
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/indexFUZZ
```

Page Fuzzing

```
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```

Recursive Fuzzing

```
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```

Sub-domain Fuzzing

```
ffuf -w wordlist.txt:FUZZ -u https://FUZZ.hackthebox.eu/
```

VHost Fuzzing

```
ffuf -w wordlist.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs xxx
```

Parameter Fuzzing - GET

```
ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
```

Parameter Fuzzing - POST

```
ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

Value Fuzzing

```
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

## dirb

Page Fuzzing:

```bash
gobuster dir -u http://$IP -w /usr/share/dirb/wordlists/common.txt -x .<ext1>,.<ext2> -o gobuster.txt
```

Extension Fuzzing:

```bash
dirb http://$IP -X .<extension> -o dirb.txt
```

# Reference

This page is **heavily** based on HackTheBox Academy Web Fuzzing:

{% embed url="https://academy.hackthebox.eu/catalogue" %}
