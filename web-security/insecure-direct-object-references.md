# Insecure Direct Object References

## Insecure Direct Object References

Insecure Direct Object References occur when an application provides direct access to objects based on user-supplied input. As a result of this vulnerability, attackers can bypass authorization and access resources in the system directly, for example database records or files. - OWASP

We may be able to access unauthorized resources if objects restrictions are not applied correctly. 

> We may be able to visit an admin panel being a low privileged user in the web application.

The value of a parameter is used directly to retrieve a database record.

```text
http://foo.bar/somepage?invoice=12345
```

The value of a parameter is used directly to perform an operation in the system

```text
http://foo.bar/changepassword?user=someuser
```

The value of a parameter is used directly to retrieve a file system resource

```text
http://foo.bar/showImage?img=img00011
```

The value of a parameter is used directly to access application functionality

```text
http://foo.bar/accessPage?menuitem=12
```

Extracted from here:

{% embed url="https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Insecure%20Direct%20Object%20References" %}



