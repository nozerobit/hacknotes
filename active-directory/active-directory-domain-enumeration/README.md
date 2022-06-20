# Active Directory Domain Enumeration

## Active Directory Domain Enumeration

When enumerating an AD server from a client make sure the user you are logged in as is a member of the AD.

## Active Directory Enumeration

### Users / Groups / Computers

We should look for users with high-priveleges across the domain e.g. Domain Admins or Derivative Local Admins and look for custom groups.

```powershell
# get all users in the domain
cmd> net user /domain
cmd> net user [username] /domain

# get all groups in the domain
cmd> net group /domain
cmd> net group [groupname] /domain

# get all computers in domain
cmd> net view
cmd> net view /domain

# get resources/shares of specified computer
cmd> net view \\[computer_name] /domain
```

Domain Controller hostname (PdcRoleOwner):

```powershell
PS> [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```

### Service Principal Names (AD Service Accounts)

A SPN is a unique name for a service on a host, used to associate with an Active Directory service account. We can enumerate SPNs to obtain the IP address and port number of apps running on servers integrated with Active Directory. Query the Domain Controller in search of SPNs.

SPN Examples

* `CIFS/MYCOMPUTER$` - file share access.
* `LDAP/MYCOMPUTER$` - querying AD info via. LDAP.
* `HTTP/MYCOMPUTER$` - Web services such as IIS.
* `MSSQLSvc/MYCOMPUTER$` - MSSQL

Tip: Perform `nslookup` of the service hostname -> see if there is an entrypoint here.

Automated SPN enumeration scripts:

```powershell
# Kerberoast: https://github.com/nidem/kerberoast/blob/master/GetUserSPNs.ps1
PS> .\GetUserSPNs.ps1

# Powershell Empire: https://github.com/compwiz32/PowerShell/blob/master/Get-SPN.ps1
PS> .\Get-SPN.ps1
```

Enumerate Logged-in users and active user sessions:

```powershell
PS> Set-ExecutionPolicy Unrestricted
PS> Import-Module .\PowerView.ps1
PS> Get-NetLoggedon -ComputerName [computer_name]    # enum logged-in users
PS> Get-NetSession -ComputerName [domain_controller] # enum active user sessions
```

### BloodHound

When we have to enumerate an Active Directory environment we should use the correct collector:

* AzureHound for Azure Active Directory
* SharpHound for local Active Directory

#### Azure Hound

```
# require: Install-Module -name Az -AllowClobber
# require: Install-Module -name AzureADPreview -AllowClobber
Connect-AzureAD
Connect-AzAccount
. .\AzureHound.ps1
Invoke-AzureHound
```

**BloodHound**

Run the collector on the machine using SharpHound.exe [https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.exe](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.exe) /usr/lib/bloodhound/resources/app/Collectors/SharpHound.exe

```
.\SharpHound.exe -c all -d active.htb -SearchForest
.\SharpHound.exe --EncryptZip --ZipFilename export.zip
.\SharpHound.exe -c all,GPOLocalGroup
.\SharpHound.exe -c all --LdapUsername <UserName> --LdapPassword <Password> --JSONFolder <PathToFile>
.\SharpHound.exe -c all -d active.htb --LdapUsername <UserName> --LdapPassword <Password> --domaincontroller 10.10.10.10
.\SharpHound.exe -c all,GPOLocalGroup --outputdirectory C:\Windows\Temp --randomizefilenames --prettyjson --nosavecache --encryptzip --collectallproperties --throttle 10000 --jitter 23
```

Alternatively, run the collector on the machine using Powershell

```
# https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.ps1
# /usr/lib/bloodhound/resources/app/Collectors/SharpHound.ps1
Invoke-BloodHound -SearchForest -CSVFolder C:\Users\Public
Invoke-BloodHound -CollectionMethod All
Invoke-BloodHound -CollectionMethod All  -LDAPUser <UserName> -LDAPPass <Password> -OutputDirectory <PathToFile>
```

Alternatively, remotely via BloodHound Python [https://github.com/fox-it/BloodHound.py](https://github.com/fox-it/BloodHound.py)

```
pip install bloodhound
bloodhound-python -d lab.local -u rsmith -p Winter2017 -gc LAB2008DC01.lab.local -c all
bloodhound-python -d victim.local -u username -p "Password" -gc machine.victim.local -c all -ns 10.10.10.11
```

Then import the zip/json files into the Neo4J database and query them.

Alternatively, we can install bloodhound with the APT package manager:

```
root@payload$ apt install bloodhound
```

Start BloodHound and the database

```
root@payload$ neo4j console
```

Alternatively, use docker

```
root@payload$ docker run -p7474:7474 -p7687:7687 -e NEO4J_AUTH=neo4j/bloodhound neo4j
```

Run bloodhound

```
root@payload$ ./bloodhound --no-sandbox
```

Go to [http://127.0.0.1:7474](http://127.0.0.1:7474), use `db:bolt://localhost:7687`

The default credentials are user:neo4J, pass:neo4j

You can add some custom queries like Bloodhound-Custom-Queries from @hausec. Replace the customqueries.json file located at `/home/username/.config/bloodhound/customqueries.json` or `C:\Users\USERNAME\AppData\Roaming\BloodHound\customqueries.json`.

### PowerView

Get Current Domain:

```
Get-NetDomain
```

Enumerate other Domains:

```
Get-NetDomain -Domain <DomainName>
```

Get Domain SID:

```
Get-DomainSID
```

Get Domain Policy:

```
Get-DomainPolicy
```

Will show us the policy configurations of the Domain about system access or Kerberos

```
(Get-DomainPolicy)."system access"
(Get-DomainPolicy)."kerberos policy"
```

#### Get Domain Controllers:

```
Get-NetDomainController
Get-NetDomainController -Domain <DomainName>
```

#### Enumerate Domain Users:

```
Get-NetUser
Get-NetUser -SamAccountName <user> 
Get-NetUser | select cn
Get-UserProperty
```

Check last password change

```
Get-UserProperty -Properties pwdlastset
```

Get a specific "string" on a user's attribute

```
Find-UserField -SearchField Description -SearchTerm "wtver"
```

Enumerate user logged on a machine

```
Get-NetLoggedon -ComputerName <ComputerName>
```

Enumerate Session Information for a machine

```
Get-NetSession -ComputerName <ComputerName>
```

Enumerate domain machines of the current/specified domain where specific users are logged into

```
Find-DomainUserLocation -Domain <DomainName> | Select-Object UserName, SessionFromName
```

#### Enum**erate** Domain Computers:

```
Get-NetComputer -FullData
Get-DomainGroup
```

#### Enumerate Live machines

```
Get-NetComputer -Ping
```

#### Enum**erate** Groups and Group Members:

```
Get-NetGroupMember -GroupName "<GroupName>" -Domain <DomainName>
```

Enumerate the members of a specified group of the domain

```
Get-DomainGroup -Identity <GroupName> | Select-Object -ExpandProperty Member
```

Returns all GPOs in a domain that modify local group memberships through Restricted Groups or Group Policy Preferences

```
Get-DomainGPOLocalGroup | Select-Object GPODisplayName, GroupName
```

#### Enumerate Shares

Enumerate Domain Shares

```
Find-DomainShare
```

Enumerate Domain Shares the current user has access

```
Find-DomainShare -CheckShareAccess
```

#### Enum**erate** Group Policies:

```
Get-NetGPO
```

Shows active Policy on the specified machine

```
Get-NetGPO -ComputerName <Name of the PC>
Get-NetGPOGroup
```

Get users that are part of a Machine's local Admin group

```
Find-GPOComputerAdmin -ComputerName <ComputerName>
```

#### Enum**erate** OUs:

```
Get-NetOU -FullData 
Get-NetGPO -GPOname <The GUID of the GPO>
```

#### Enum**erate** ACLs:

Returns the ACLs associated with the specified account

```
Get-ObjectAcl -SamAccountName <AccountName> -ResolveGUIDs
Get-ObjectAcl -ADSprefix 'CN=Administrator, CN=Users' -Verbose
```

Search for interesting ACEs

```
Invoke-ACLScanner -ResolveGUIDs
```

Check the ACLs associated with a specified path (e.g smb share)

```
Get-PathAcl -Path "\\Path\Of\A\Share"
```

#### Enum**erate** Domain Trust:

```
Get-NetDomainTrust
Get-NetDomainTrust -Domain <DomainName>
```

#### Enum**erate** Forest Trust:

```
Get-NetForestDomain
Get-NetForestDomain Forest <ForestName>
```

Domains of Forest Enumeration

```
Get-NetForestDomain
Get-NetForestDomain Forest <ForestName>
```

Map the Trust of the Forest

```
Get-NetForestTrust
Get-NetDomainTrust -Forest <ForestName>
```

#### User Hunting

Finds all machines on the current domain where the current user has local admin access

```
Find-LocalAdminAccess -Verbose
```

Find local admins on all machines of the domain:

```
Invoke-EnumerateLocalAdmin -Verbose
```

Find computers where a Domain Admin OR a specified user has a session

```
Invoke-UserHunter
Invoke-UserHunter -GroupName "RDPUsers"
Invoke-UserHunter -Stealth
```

Confirming admin access:

```
Invoke-UserHunter -CheckAccess
```

### AD Module

**Get Current Domain:**

```
Get-ADDomain
```

**Enumerate Other Domains:**

```
Get-ADDomain -Identity <Domain>
```

**Get Domain SID:**

```
Get-DomainSID
```

**Get Domain Controllers:**

```
Get-ADDomainController
Get-ADDomainController -Identity <DomainName>
```

**Enumerate Domain Users:**

```
Get-ADUser -Filter * -Identity <user> -Properties *
```

**Get a specific "string" on a user's attribute:**

```
Get-ADUser -Filter 'Description -like "*wtver*"' -Properties Description | select Name, Description
```

**Enumerate Domain Computers:**

```
Get-ADComputer -Filter * -Properties *
Get-ADGroup -Filter *
```

**Enumerate Domain Trust:**

```
Get-ADTrust -Filter *
Get-ADTrust -Identity <DomainName>
```

**Enumerate Forest Trust:**

```
Get-ADForest
Get-ADForest -Identity <ForestName>
```

**Domains of Forest Enumeration:**

```
(Get-ADForest).Domains
```

**Enum Local AppLocker Effective Policy:**

```
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

### Useful Enumeration Tools

* [ldapdomaindump](https://github.com/dirkjanm/ldapdomaindump) Information dumper via LDAP
* [adidnsdump](https://github.com/dirkjanm/adidnsdump) Integrated DNS dumping by any authenticated user
* [ACLight](https://github.com/cyberark/ACLight) Advanced Discovery of Privileged Accounts
* [ADRecon](https://github.com/sense-of-security/ADRecon) Detailed Active Directory Recon Tool

## References

{% embed url="https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md" %}

{% embed url="https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet#domain-enumeration" %}

