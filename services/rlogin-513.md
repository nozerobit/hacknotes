# Rlogin - 513

## Rlogin Information

This service was mostly used in the old days for remote administration but now because of security issues this service has been replaced by the slogin and the ssh.

Default port: 513

```text
PORT    STATE SERVICE
513/tcp open  login
```

## Login

```text
apt-get install rsh-client
```

Using the login name root \(no password is required for this service\), authenticate to the remote host:

```text
rlogin <IP> -l <username>
```

