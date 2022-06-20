# XPATH Injection

## XML Information

XML \(Extensible Markup Language\) v1.0 is a markup language \(similar to HTML\) that is primarily used to describe data rather than to display it. XML documents are frequently utilized as databases due to their nature. Queries can read and write data, and the XML database looks exactly like an XML document.

XML Declaration, the XML declaration defines the current version number \(1.0 in our case\) and the encoding type:

```markup
<?xml version="1.0" encoding="ISO-8859-1"?>
```

Although it is not required, if it exists, it must be the first line in the document.

In contrast to HTML, you can use any naming convention you wish for elements, just as long as you follow these simple naming rules:

* Names must start with a letter or underscore and cannot start with the letters xml 
* Names are case sensitive 
* Names can contain letters, digits, periods but no spaces

## XPATH Information

XPath \(XML Path Language\) is a standard language used to query and navigate XML documents. At the time of this writing, the latest version is 3.0. XPath makes use of path expressions to select nodes from an XML document.

### XPATH Vulnerable Code

This is a very simple example of a plaintext password insecure XML database that contains user credentials:

```markup
<?xml version="1.0" encoding="ISO-8859-1"?> 
<users> 
    <user id='1'> 
        <username>wixnic</username> <!-- Comment --> 
        <password>soVulnerable</password> 
    </user> 
    <user id='2'> 
        <username>wixnic_admin</username> 
        <password>really?</password> 
    </user> 
</users>
```

The XML declaration appears on the first line of the document. The current version number \(in our example, 1.0\) and the encoding type are normally defined in the XML declaration.

> Note that all elements must have a closing tag and must be properly nested.

The document's root element is described in the following line. XML elements define the document's structure and designate named sections of data. It's vital to remember that elements create a document tree and that `<users>` is the parent of all other elements in this case. A child element is described in the following line. Elements, like HTML tags, can have attributes \(name="value"\) that help define the qualities of the element. It's crucial to remember that attributes must be quoted and can only be used in start tags. The id attribute on the user element has a value of 1 in our example.

This text can be regarded as the element's value. Wixnic would be the text included in the table users, column user with id=1 in a database schema.

An example of an XPath query to look for a node user based on the inserted credentials can be this:

```markup
//user[username/text()='<USERNAME>' and password/text()='<PASSWORD>']
```

Where `<USERNMAE>` and `<PASSWORD>` are user-supplied input values, they should be sanitized before use.





