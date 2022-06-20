# Insecure Deserialization

### JWT

### Edit JWT

We can use this website to modify a JWT token:

{% embed url="https://jwt.io/" %}

### Keys

If the JWT is encrypted with a signature but we can modify the signature then we may be able to generate our own private key with openssl:

```text
openssl genrsa -out privKey.key 2048
```

### Deserialization Exploits

It is possible to have remote code execution with deserialization as seen [here](https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/).

