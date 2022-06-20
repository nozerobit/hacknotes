# Network Data Management Protocol \(ndmp\) - 10000

## **NDMP Information** <a id="protocol-information"></a>

**NDMP**, or **Network Data Management Protocol**, is a protocol meant to transport data between network-attached storage \([NAS](https://en.wikipedia.org/wiki/Network-attached_storage)\) devices and [backup](https://en.wikipedia.org/wiki/Backup) devices. This removes the need for transporting the data through the backup server itself, thus enhancing the speed and removing the load from the backup server. Extracted from [Wikipedia](https://en.wikipedia.org/wiki/NDMP).

**Default port:** 10000

```text
PORT      STATE SERVICE REASON  VERSION
10000/tcp open  ndmp    syn-ack Symantec/Veritas Backup Exec ndmp
```

## **Enumeration** <a id="enumeration"></a>

We can enumerate the NDMP file system information and its version with:

```text
nmap -n -sV --script "ndmp-fs-info or ndmp-version" -p 10000 <IP>
```



