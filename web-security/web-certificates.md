# Web Certificates

## Web Certificates

A website security certificate is essentially a digital stamp of approval from a certificate authority, which is an industry-trusted third party \(CA\). It's a digital file comprising information issued by a CA that certifies that a website is secure when accessed through an encrypted connection.

### Self-Signed Certificates

Self-Signed certificates may be dangerous but as long as you know from who and which data is in the certificate then you may trust it.

We can generate a self-signed certificate with the following command:

```text
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
```

You can also add `-nodes` \(short for `no DES`\) if you don't want to protect your private key with a passphrase. Otherwise it will prompt you for "at least a 4 character" password.

The `days` parameter \(365\) you can replace with any number to affect the expiration date. It will then prompt you for things like "Country Name", but you can just hit Enter and accept the defaults.

Add `-subj '/CN=localhost'` to suppress questions about the contents of the certificate \(replace `localhost` with your desired domain\).

Self-signed certificates are not validated with any third party unless you import them to the browsers previously. If you need more security, you should use a certificate signed by a [certificate authority](https://en.wikipedia.org/wiki/Certificate_authority) \(CA\).

{% embed url="https://stackoverflow.com/questions/10175812/how-to-generate-a-self-signed-ssl-certificate-using-openssl" %}

### OpenSSL

We can enumerate the certificate of a remote server with:

```text
openssl s_client -connect <IP:PORT>
```

