# Directory Traversal

## Directory Traversal Information

Directory traversal \(sometimes called file path traversal\) is a web security flaw that allows an attacker to access arbitrary files on a server that is hosting an application. This could comprise application code and data, back-end system credentials, and critical operating system files, among other things. An attacker may be able to write to arbitrary files on the server in some instances, allowing them to change application data or behavior and eventually gain complete control of the server.

## Vulnerable Code

## Directory Traversal/File Path Traversal

### Slashes

**Unix/Linux**

* Slash /

**Windows**

* Slash / 
* Backslash \

A specific sequence can be used to terminate the current file name. This sequence takes the name of NULL BYTE:

> Note that %00 does not work with PHP versions &gt;= 5.3.4.

### Common Obstacles to exploiting File Path Traversal Vulnerabilities <a id="common-obstacles-to-exploiting-file-path-traversal-vulnerabilities"></a>

Many applications that place user input into file paths implement some kind of defense against path traversal attacks, and these can often be circumvented.

If an application strips or blocks directory traversal sequences from the user-supplied filename, then it might be possible to bypass the defense using a variety of techniques.

You might be able to use an absolute path from the filesystem root, such as `filename=/etc/passwd`, to directly reference a file without using any traversal sequences.

You might be able to use nested traversal sequences, such as `....//` or `....\/`, which will revert to simple traversal sequences when the inner sequence is stripped.

You might be able to use various non-standard encodings, such as `..%c0%af` or `..%252f`, to bypass the input filter.

If an application requires that the user-supplied filename must start with the expected base folder, such as `/var/www/images`, then it might be possible to include the required base folder followed by suitable traversal sequences. For example:

`filename=/var/www/images/../../../etc/passwd`

If an application requires that the user-supplied filename must end with an expected file extension, such as `.png`, then it might be possible to use a null byte to effectively terminate the file path before the required extension. For example:

`filename=../../../etc/passwd%00.png`

## Curl for Directory Traversal

Curl will correct routes with directory traversal and remove the `../` by default. We can get what we're searching for if we use the `—path-as-is` flag. The following is taken from the man pages:

> Man Page: When using `–path-as-is` Curl will ignore any `/../` or `/./` sequences in the specified URL path. Curl will normally squash or merge them according to standards, but with this option enabled, we can tell it not to do it.

```bash
curl --path-as-is http://10.10.10.10:4433/../../../../etc/lsb-release
```


