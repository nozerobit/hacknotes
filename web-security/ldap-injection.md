# LDAP Injection

## LDAP Information

Lightweight Directory Access Protocol \(LDAP\) is an acronym for Lightweight Directory Access Protocol. It's a TCP/IP protocol for modifying and querying directory services. The directory service is a database-like virtual storage system that organizes data into hierarchical structures. The LDAP structure is based on a directory tree with entries.

Because LDAP is object-oriented, every entry in an LDAP directory service is an instance of an object that must adhere to the rules established for the object's attributes. LDAP can be used to manage and authenticate users as well as query objects from a directory database. It's important to remember that LDAP is only a protocol for accessing Directory Service, not a storage mechanism in and of itself.

Although LDAP is used to interface with Directory Databases, it does not have any storing capabilities as a protocol. Microsoft Active Directory \(where LDAP is frequently used in the authentication process\) and the less well-known OpenLDAP are two examples of databases that use directory structures.

LDIF stands for LDAP Data Interchange Format, and it is used to store objects in directory databases accessed via LDAP. The LDIF standard describes directory content as a collection of records, one for each object \(or entry\). Update requests such as Add, Modify, Delete, and Rename is likewise represented as a group of records, with one record for each update request.

By declaring its assumptions in an LDIF file, a directory database can support LDIF. It might just be a plaintext file with directory data representation and LDAP instructions. They can also read, write, and update information in a directory.

## LDAP Syntax

LDAP is a protocol that has its own structure for querying the back-end database. It utilizes operators like the following:

* "=" \(equal to\) 
* \| \(logical or\) 
* ! \(logical not\) 
* & \(logical and\) 
* \* \(wildcard\) – stands for any string or character

In larger expressions, these operators are utilized \(LDAP queries\). Examples of LDAP queries can be found below. They're talking about the database schema from the prior module.

* \(cn=Jason\) will fetch personal entries where canonical name is "Jason” 

LDAP query expressions can also be concatenated, resulting in a sample query like the one below:

```text
(|(sn=a*)(cn=b*))
```

The first OR operator is used in this example to indicate that we are looking for all records whose surname begins with "a" or whose canonical name begins with "b."

### LDAP over TCP

In order to connect to standalone LDAP services via the TCP protocol, you can use tool named JXplorer.

```text
java -jar JXplorer.jar
```

LDAP can be integrated with a web application, which can take user input and implement it into an LDAP query. If there is no sanitization of user input, several things can go wrong.

### Warning

Pulling a huge amount of data at once could cause a Denial of Service issue; if the back-end database is large enough, the front-end was most likely intended to filter query results in order to avoid overloading the database engine. Multiple wildcard queries in this situation may cause the database to become inaccessible, thereby blocking access to the application.



