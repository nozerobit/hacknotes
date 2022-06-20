# Apple Filing Protocol \(AFP\) - 548

## AFP Information

The Apple Filing Protocol \(AFP\), formerly known as the AppleTalk Filing Protocol, is a proprietary network protocol that is part of the Apple File Service \(AFS\) and provides file services for macOS and the classic Mac OS. AFP is one of several file services supported by macOS. AFP currently supports Unicode file names, POSIX and access control list permissions, resource forks, named extended attributes, and file descriptors.

Default port: 548

```text
PORT    STATE SERVICE
548/tcp open  afp
```

## Enumeration

```text
nmap -sV --script "afp-* and not dos and not brute" -p <PORT> <IP>
```

