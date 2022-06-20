# TFTP/Bittorrent-tracker - 69/UDP

## TFTP Information <a id="basic-information"></a>

**TFTP** uses UDP port 69 and **requires no authentication**—clients read from, and write to servers using the datagram format outlined in RFC 1350. Due to deficiencies within the protocol \(namely lack of authentication and no transport security\), it is uncommon to find servers on the public Internet. Within large internal networks, however, TFTP is used to serve configuration files and ROM images to VoIP handsets and other devices.

**Default Port:** 69/UDP

```text
PORT   STATE SERVICE REASON
69/udp open  tftp    script-set
```

## Enumeration <a id="enumeration"></a>

TFTP doesn't provide directory listing so the script `tftp-enum` from `nmap` will try to brute-force default paths.

```text
nmap -n -Pn -sU -p69 -sV --script tftp-enum <IP>
```

## TFTP Binary Mode

When we need to upload binary files (anything non-text) via TFTP, it’s important to switch to binary mode. Otherwise the binaries will be corrupted during the transfer.

There are two (2) main modes:

1. netascii (ASCII): Used to transfer text files
2. binary: Used to transfer binary files

By default it will be in ASCII mode which is used to transfer text files.

We can determine the mode using the following command:

```
tftp> status
Connected to 10.10.10.90.
Mode: netascii Verbose: off Tracing: off
Rexmt-interval: 5 seconds, Max-timeout: 25 s
```

We can change the mode by doing the following:

```
tftp> binary
tftp> status
Connected to 10.10.10.90.
Mode: octet Verbose: off Tracing: off
Rexmt-interval: 5 seconds, Max-timeout: 25 seconds
```

## TFTP Upload/Download

We can upload a file:

```
tftp> put test.txt
```

We can download a file:

```
tftp> get test.txt
```

When using a path it will look like this:

```
tftp> put nc.exe \windows\system32\nc.exe
tftp> get \windows\system32\eula.txt
```

> Note: The files `\windows\system32\license.rtf` and/or `\windows\system32\eula.txt` will help us determine the Windows OS version.

