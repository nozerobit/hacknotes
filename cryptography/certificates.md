# Certificates

## Certificate Authority

&#x20;In [cryptography](https://en.wikipedia.org/wiki/Cryptography), a **certificate authority** or **certification authority** (**CA**) is an entity that issues [digital certificates](https://en.wikipedia.org/wiki/Public\_key\_certificate). A digital certificate certifies the ownership of a public key by the named subject of the certificate. This allows others (relying parties) to rely upon signatures or on assertions made about the private key that corresponds to the certified public key. A CA acts as a trusted third party—trusted both by the subject (owner) of the certificate and by the party relying upon the certificate. The format of these certificates is specified by the [X.509](https://en.wikipedia.org/wiki/X.509) or [EMV](https://en.wikipedia.org/wiki/EMV) standard.

The text above was extracted from [Wikipedia](https://en.wikipedia.org/wiki/Certificate\_authority).

## Microsoft Active Directory Certificate Services

### Request a Certificate

This webpage will allow **us** to generate a certificate that we can use to authenticate as a user. There are two ways to get the two files we need, a key (.key) and a certificate (.crt or .cer, they are interchangeable).

![](<../.gitbook/assets/image (9).png>)

Alternatively and the MOST recommended way is to create our own certificate:

```aspnet
❯ openssl genrsa -aes256 -out filename.key 2048

Generating RSA private key, 2048 bit long modulus (2 primes)
.......................................................+++++
................+++++
e is 65537 (0x010001)
Enter pass phrase for filename.key:
Verifying - Enter pass phrase for filename.key:

❯ openssl req -new -key filename.key -out filename.csr
Enter pass phrase for filename.key:
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

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

❯ cat filename.csr
-----BEGIN CERTIFICATE REQUEST-----
MIICijCCAXICAQAwRTELMAkGA1UEBhMCQVUxEzARBgNVBAgMClNvbWUtU3RhdGUx
ITAfBgNVBAoMGEludGVybmV0IFdpZGdpdHMgUHR5IEx0ZDCCASIwDQYJKoZIhvcN
AQEBBQADggEPADCCAQoCggEBAK+fXSxTv3s7Jij08HLfTnzBRxuBgPqHVX6ID2k9
wDmncz1QQW2+9AI0D4jDQcywVoeatc1CPu/cKSbtXuz31GPs5KRgrQSyFBH2z2Z2
kEAvGq7so9wJKF1iHtQfuVJqTcoeo37vtbwOsKvMtzmx8uLJR4nqFSCAiqYifpOh
GemVHMv/zhLj0P2pR1tpZqPCg4ITbWZ5oIohv87+GC7fmZPWXMLYS/OoV48q3ZvY
S1SZTeeLZwEyBiV2nlLpLK2eRGMJdrQQXcGdMVEQhfEvqsExp4nzG/ZsLtvVef8t
rqUBlL1XY7oSWgecW/DQdOMeVpgxSR5OXt2d/0NCdIl7oSsCAwEAAaAAMA0GCSqG
SIb3DQEBCwUAA4IBAQCPe0ohDn9ULoTwjbDcZ4vurrQ98s6Fw0r56W/WLM6VAhfC
1SsofXqSt4VUlFyHxHCt7ehFfr0Lp5Xk12pbtEpI9S0U/8jdiniP1IdcJ4llE9wX
vXA+U5qwn8pb9T5xgHJht0v2nvH/a48Sy9oDU0PBOX0bvjHpHCJw3aI2HGDurSG/
gm6ToIPEO4e8twT0Pa/WcPUnAXdRmw0cTwC/9C3DLMpkVch3UiAwxXKVNma8Agft
d0z/8LnrnxUSdu5WL038Dyl4sAgd3hmussiQcwF7ZAInNao09Rfi2IG7Zi5+/WQL
VMe0Q0Zmsk9LD9w3q9jLeztiMQqgH7FrQAQAejKw
-----END CERTIFICATE REQUEST-----
```

We'll click on advanced certificate request:

![](<../.gitbook/assets/image (3).png>)

Then we'll paste the `filename.csr` certificate request into the text box:

![](<../.gitbook/assets/image (5).png>)

Click submit and wait.

![](<../.gitbook/assets/image (4).png>)

After the certificate is issued, we can download it in one of the encodings above. Click on Download certificate.&#x20;

Now we have our certificate:

```
❯ openssl x509 -in /home/kali/Downloads/certnew.cer -text
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            69:00:00:00:15:32:76:07:9f:ff:49:e3:bd:00:00:00:00:00:15
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: DC = LOCAL, DC = HTB, CN = HTB-SIZZLE-CA
        Validity
            Not Before: Aug 12 19:14:46 2021 GMT
            Not After : Aug 12 19:14:46 2022 GMT
        Subject: DC = LOCAL, DC = HTB, CN = Users, CN = amanda
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:af:9f:5d:2c:53:bf:7b:3b:26:28:f4:f0:72:df:
                    4e:7c:c1:47:1b:81:80:fa:87:55:7e:88:0f:69:3d:
                    c0:39:a7:73:3d:50:41:6d:be:f4:02:34:0f:88:c3:
                    41:cc:b0:56:87:9a:b5:cd:42:3e:ef:dc:29:26:ed:
                    5e:ec:f7:d4:63:ec:e4:a4:60:ad:04:b2:14:11:f6:
                    cf:66:76:90:40:2f:1a:ae:ec:a3:dc:09:28:5d:62:
                    1e:d4:1f:b9:52:6a:4d:ca:1e:a3:7e:ef:b5:bc:0e:
                    b0:ab:cc:b7:39:b1:f2:e2:c9:47:89:ea:15:20:80:
                    8a:a6:22:7e:93:a1:19:e9:95:1c:cb:ff:ce:12:e3:
                    d0:fd:a9:47:5b:69:66:a3:c2:83:82:13:6d:66:79:
                    a0:8a:21:bf:ce:fe:18:2e:df:99:93:d6:5c:c2:d8:
                    4b:f3:a8:57:8f:2a:dd:9b:d8:4b:54:99:4d:e7:8b:
                    67:01:32:06:25:76:9e:52:e9:2c:ad:9e:44:63:09:
                    76:b4:10:5d:c1:9d:31:51:10:85:f1:2f:aa:c1:31:
                    a7:89:f3:1b:f6:6c:2e:db:d5:79:ff:2d:ae:a5:01:
                    94:bd:57:63:ba:12:5a:07:9c:5b:f0:d0:74:e3:1e:
                    56:98:31:49:1e:4e:5e:dd:9d:ff:43:42:74:89:7b:
                    a1:2b
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                E1:0C:28:D9:9F:FC:4A:6B:D1:C0:FB:20:ED:4D:20:15:36:D7:D9:DF
            X509v3 Authority Key Identifier:
                keyid:40:06:E4:54:B3:37:98:BC:22:2E:0E:19:36:0A:18:A0:B1:DE:0B:8A

            X509v3 CRL Distribution Points:

                Full Name:
                  URI:ldap:///CN=HTB-SIZZLE-CA,CN=sizzle,CN=CDP,CN=Public%20Key%20Services,CN=Services,CN=Configuration,DC=HTB,DC=LOCAL?certificateRevocationList?base?objectClass=cRLDistributionPoint

            Authority Information Access:
                CA Issuers - URI:ldap:///CN=HTB-SIZZLE-CA,CN=AIA,CN=Public%20Key%20Services,CN=Services,CN=Configuration,DC=HTB,DC=LOCAL?cACertificate?base?objectClass=certificationAuthority

            1.3.6.1.4.1.311.20.2:
                ...U.s.e.r
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Extended Key Usage:
                Microsoft Encrypted File System, E-mail Protection, TLS Web Client Authentication
            X509v3 Subject Alternative Name:
                othername:<unsupported>
            S/MIME Capabilities:
......0...+....0050...*.H..
..*.H..
    Signature Algorithm: sha256WithRSAEncryption
         45:d5:80:0a:5c:ea:28:21:5b:c9:ad:46:c9:af:1e:da:8c:36:
         81:4c:50:b6:c4:8c:27:47:87:8a:dc:08:92:c4:aa:aa:f9:c0:
         56:0a:66:d8:de:b3:22:2a:71:4c:a3:2a:33:19:56:0d:12:dd:
         7d:b9:11:5c:f3:68:b9:dc:a5:e0:4e:10:27:9b:46:9e:14:40:
         c3:d2:ae:5b:7f:c5:3c:9c:84:02:8e:0f:be:a4:c4:01:5a:36:
         ce:0b:0e:86:8f:bc:37:ea:61:16:35:9d:7e:54:f3:68:6a:91:
         da:56:86:e3:63:08:0c:c9:a8:2f:3e:f4:7e:64:ba:d2:d2:d1:
         ba:0d:73:2a:48:9e:5a:22:91:40:ca:83:fe:ff:09:48:7f:e8:
         35:27:49:c4:8c:b7:4e:3d:b9:93:75:d4:40:e5:60:49:8f:f2:
         14:c8:90:cb:4d:f8:12:f0:98:a1:89:03:df:2c:bd:89:fe:1e:
         aa:a6:bb:84:2c:9f:d0:6b:fa:e4:0a:fd:00:d7:46:db:4c:68:
         d4:12:e8:da:61:38:87:86:4c:4d:25:23:2c:48:5f:18:d0:af:
         3d:b8:12:79:d3:bb:7f:ac:28:b0:59:a6:7c:75:36:d6:94:2b:
         2a:ae:a6:2c:b4:86:66:cd:4f:31:d3:19:7c:62:28:5c:b3:18:
         a1:ba:7f:80
-----BEGIN CERTIFICATE-----
MIIFtjCCBJ6gAwIBAgITaQAAABUydgef/0njvQAAAAAAFTANBgkqhkiG9w0BAQsF
ADBEMRUwEwYKCZImiZPyLGQBGRYFTE9DQUwxEzARBgoJkiaJk/IsZAEZFgNIVEIx
FjAUBgNVBAMTDUhUQi1TSVpaTEUtQ0EwHhcNMjEwODEyMTkxNDQ2WhcNMjIwODEy
MTkxNDQ2WjBNMRUwEwYKCZImiZPyLGQBGRYFTE9DQUwxEzARBgoJkiaJk/IsZAEZ
FgNIVEIxDjAMBgNVBAMTBVVzZXJzMQ8wDQYDVQQDEwZhbWFuZGEwggEiMA0GCSqG
SIb3DQEBAQUAA4IBDwAwggEKAoIBAQCvn10sU797OyYo9PBy3058wUcbgYD6h1V+
iA9pPcA5p3M9UEFtvvQCNA+Iw0HMsFaHmrXNQj7v3Ckm7V7s99Rj7OSkYK0EshQR
9s9mdpBALxqu7KPcCShdYh7UH7lSak3KHqN+77W8DrCrzLc5sfLiyUeJ6hUggIqm
In6ToRnplRzL/84S49D9qUdbaWajwoOCE21meaCKIb/O/hgu35mT1lzC2EvzqFeP
Kt2b2EtUmU3ni2cBMgYldp5S6SytnkRjCXa0EF3BnTFREIXxL6rBMaeJ8xv2bC7b
1Xn/La6lAZS9V2O6EloHnFvw0HTjHlaYMUkeTl7dnf9DQnSJe6ErAgMBAAGjggKW
MIICkjAdBgNVHQ4EFgQU4Qwo2Z/8SmvRwPsg7U0gFTbX2d8wHwYDVR0jBBgwFoAU
QAbkVLM3mLwiLg4ZNgoYoLHeC4owgcgGA1UdHwSBwDCBvTCBuqCBt6CBtIaBsWxk
YXA6Ly8vQ049SFRCLVNJWlpMRS1DQSxDTj1zaXp6bGUsQ049Q0RQLENOPVB1Ymxp
YyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24s
REM9SFRCLERDPUxPQ0FMP2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9v
YmplY3RDbGFzcz1jUkxEaXN0cmlidXRpb25Qb2ludDCBvQYIKwYBBQUHAQEEgbAw
ga0wgaoGCCsGAQUFBzAChoGdbGRhcDovLy9DTj1IVEItU0laWkxFLUNBLENOPUFJ
QSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25m
aWd1cmF0aW9uLERDPUhUQixEQz1MT0NBTD9jQUNlcnRpZmljYXRlP2Jhc2U/b2Jq
ZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTAXBgkrBgEEAYI3FAIECh4I
AFUAcwBlAHIwDgYDVR0PAQH/BAQDAgWgMCkGA1UdJQQiMCAGCisGAQQBgjcKAwQG
CCsGAQUFBwMEBggrBgEFBQcDAjArBgNVHREEJDAioCAGCisGAQQBgjcUAgOgEgwQ
YW1hbmRhQEhUQi5MT0NBTDBEBgkqhkiG9w0BCQ8ENzA1MA4GCCqGSIb3DQMCAgIA
gDAOBggqhkiG9w0DBAICAIAwBwYFKw4DAgcwCgYIKoZIhvcNAwcwDQYJKoZIhvcN
AQELBQADggEBAEXVgApc6ighW8mtRsmvHtqMNoFMULbEjCdHh4rcCJLEqqr5wFYK
ZtjesyIqcUyjKjMZVg0S3X25EVzzaLncpeBOECebRp4UQMPSrlt/xTychAKOD76k
xAFaNs4LDoaPvDfqYRY1nX5U82hqkdpWhuNjCAzJqC8+9H5kutLS0boNcypInloi
kUDKg/7/CUh/6DUnScSMt049uZN11EDlYEmP8hTIkMtN+BLwmKGJA98svYn+Hqqm
u4Qsn9Br+uQK/QDXRttMaNQS6NphOIeGTE0lIyxIXxjQrz24EnnTu3+sKLBZpnx1
NtaUKyqupiy0hmbNTzHTGXxiKFyzGKG6f4A=
-----END CERTIFICATE-----
```

## Generate Private Key

### OpenSSL

We can generate an openssl private key with:

```
openssl genrsa -aes256 -out filename.key 2048
openssl genrsa -des3 -out amanda.key 2048
```

## Sign a Certificate

We can do a certificate signing request with:

```
openssl req -new -key filename.key -out filename.csr
```

