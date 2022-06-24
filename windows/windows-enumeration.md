# Windows Enumeration

## Users and Groups

See the current user:

```
C:\Users\low>whoami
desktop-l3rjjkv\low
```

Enumerate a specific user:

```
C:\Users\low>net user low
User name                    low
Full Name
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            1/12/2022 5:17:56 AM
Password expires             Never
Password changeable          1/12/2022 5:17:56 AM
Password required            No
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   1/12/2022 5:17:44 AM

Logon hours allowed          All

Local Group Memberships      *Administrators
Global Group memberships     *None
The command completed successfully.
```

Enumerate all the local users:

```
C:\Users\low>net user

User accounts for \\DESKTOP-L3RJJKV

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
low                      WDAGUtilityAccount
The command completed successfully.
```

## Hostname

Enumerate the hostname:

```
C:\Users\low>hostname
DESKTOP-L3RJJKV
```

## Operating System Version and Architecture

Enumerate the OS version and architecture:

```
C:\Users\low>systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
OS Name:                   Microsoft Windows 11 Pro
OS Version:                10.0.22000 N/A Build 22000
System Type:               x64-based PC
```

## Running Processes and Services

Enumerate processes that are part of a service, this doesn't list processes run by privileged users:

```
C:\Users\low>tasklist /SVC

Image Name                     PID Services
========================= ======== ============================================
System Idle Process              0 N/A
System                           4 N/A
Registry                       100 N/A
smss.exe                       352 N/A
csrss.exe                      464 N/A
<...SNIP...>
```

## Network Information

Enumerate all the network interfaces:

```
C:\Users\low>ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-L3RJJKV
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet0:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . : localdomain
   Description . . . . . . . . . . . : Intel(R) 82574L Gigabit Network Connection
   Physical Address. . . . . . . . . : 00-0C-29-A6-07-59
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
```

Enumerate the network routing table:

```
C:\Users\low>route print
===========================================================================
Interface List
  6...00 0c 29 a6 07 59 ......Intel(R) 82574L Gigabit Network Connection
  1...........................Software Loopback Interface 1
===========================================================================

IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
===========================================================================
Persistent Routes:
  None

IPv6 Route Table
===========================================================================
Active Routes:
 If Metric Network Destination      Gateway
  1    331 ::1/128                  On-link
  1    331 ff00::/8                 On-link
===========================================================================
Persistent Routes:
  None
```

Enumerate the active network connections:

```
C:\Users\low>netstat -ano

Active Connections

  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       924
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:5040           0.0.0.0:0              LISTENING       4396
  TCP    0.0.0.0:7680           0.0.0.0:0              LISTENING       8260
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING       672
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING       532
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING       1180
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING       1660
  TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING       2300
  TCP    0.0.0.0:49669          0.0.0.0:0              LISTENING       608
  TCP    [::]:135               [::]:0                 LISTENING       924
  TCP    [::]:445               [::]:0                 LISTENING       4
  TCP    [::]:7680              [::]:0                 LISTENING       8260
  TCP    [::]:49664             [::]:0                 LISTENING       672
  TCP    [::]:49665             [::]:0                 LISTENING       532
  TCP    [::]:49666             [::]:0                 LISTENING       1180
  TCP    [::]:49667             [::]:0                 LISTENING       1660
  TCP    [::]:49668             [::]:0                 LISTENING       2300
  TCP    [::]:49669             [::]:0                 LISTENING       608
  UDP    0.0.0.0:123            *:*                                    10392
  UDP    0.0.0.0:5050           *:*                                    4396
  UDP    127.0.0.1:1900         *:*                                    5564
  UDP    127.0.0.1:52035        *:*                                    5564
  UDP    127.0.0.1:63999        127.0.0.1:63999                        2628
  UDP    [::]:123               *:*                                    10392
  UDP    [::1]:1900             *:*                                    5564
  UDP    [::1]:52034            *:*                                    5564
```

## Firewalls Status and Rules

Enumerate the current firewall profile:

```
C:\Users\low>netsh advfirewall show currentprofile

Public Profile Settings:
----------------------------------------------------------------------
State                                 ON
Firewall Policy                       BlockInbound,AllowOutbound
LocalFirewallRules                    N/A (GPO-store only)
LocalConSecRules                      N/A (GPO-store only)
InboundUserNotification               Enable
RemoteManagement                      Disable
UnicastResponseToMulticast            Enable

Logging:
LogAllowedConnections                 Disable
LogDroppedConnections                 Disable
FileName                              %systemroot%\system32\LogFiles\Firewall\pfirewall.log
MaxFileSize                           4096

Ok.
```

Enumerate the firewall rules:

```
C:\Users\low>netsh advfirewall firewall show rule name=all

Rule Name:                            Microsoft Solitaire Collection
----------------------------------------------------------------------
Enabled:                              Yes
Direction:                            In
Profiles:                             Domain,Private
Grouping:                             Microsoft Solitaire Collection
LocalIP:                              Any
RemoteIP:                             Any
Protocol:                             Any
Edge traversal:                       No
Action:                               Allow

Rule Name:                            Microsoft Solitaire Collection
----------------------------------------------------------------------
Enabled:                              Yes
Direction:                            Out
Profiles:                             Domain,Private,Public
Grouping:                             Microsoft Solitaire Collection
LocalIP:                              Any
RemoteIP:                             Any
Protocol:                             Any
Edge traversal:                       No
Action:                               Allow

Rule Name:                            @{Microsoft.SecHealthUI_1000.22000.1.0_neutral__8wekyb3d8bbwe?ms-resource://Microsoft.SecHealthUI/resources/PackageDisplayName}
----------------------------------------------------------------------
Enabled:                              Yes
Direction:                            Out
Profiles:                             Domain,Private,Public
Grouping:                             Windows Security
LocalIP:                              Any
RemoteIP:                             Any
Protocol:                             Any
Edge traversal:                       No
Action:                               Allow

Rule Name:                            @{Microsoft.StorePurchaseApp_12109.1001.10.0_x64__8wekyb3d8bbwe?ms-resource://Microsoft.StorePurchaseApp/Resources/DisplayTitle}
---------------------------------------------------------------------
<...SNIP...>
```

## Process Dump

We can the information of a process with procdump.exe from Sysinternals.

```powershell
*Evil-WinRM* PS C:\Users\Chase\Documents> .\procdump64 -ma 6252 -accepteula

ProcDump v9.0 - Sysinternals process dump utility
Copyright (C) 2009-2017 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

[02:54:30] Dump 1 initiated: C:\Users\Chase\Documents\firefox.exe_190823_025430.dmp
[02:54:30] Dump 1 writing: Estimated dump file size is 280 MB.
[02:54:32] Dump 1 complete: 281 MB written in 2.1 seconds
[02:54:33] Dump count reached.
```

Alternatively, we can use PowerSploit’s Out-Minidump:

```powershell
*Evil-WinRM* PS C:\users\chase\appdata\local\temp> Out-Minidump.ps1
*Evil-WinRM* PS C:\users\chase\appdata\local\temp> menu

   ___ __ __  ____  _
  /  _]  |  ||    || |
 /  [_|  |  | |  | | |
|    _]  |  | |  | | |___
|   [_|  :  | |  | |     |
|     |\   /  |  | |     |
|_____| \_/  |____||_____|

 __    __  ____  ____   ____   ___ ___
|  |__|  ||    ||    \ |    \ |   |   |
|  |  |  | |  | |  _  ||  D  )| _   _ |
|  |  |  | |  | |  |  ||    / |  \_/  |
|  `  '  | |  | |  |  ||    \ |   |   |
 \      /  |  | |  |  ||  .  \|   |   |
  \_/\_/  |____||__|__||__|\_||___|___|


                           By: CyberVaca@HackPlayers

[+] Invoke-Binary
[+] l04d3r-LoadDll
[+] Out-Minidump

*Evil-WinRM* PS C:\users\chase\appdata\local\temp> get-process -id 6252 | Out-Minidump


    Directory: C:\users\chase\appdata\local\temp


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/23/2019  11:09 PM      286666409 firefox_6252.dmp
```

Mimikittenz is a tool that can dump process that may have passwords:

```powershell
*Evil-WinRM* PS C:\Users\Chase\Documents> Invoke-mimikittenz
───▐▀▄──────▄▀▌───▄▄▄▄▄▄▄─────────────
───▌▒▒▀▄▄▄▄▀▒▒▐▄▀▀▒██▒██▒▀▀▄──────────
──▐▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▀▄────────
──▌▒▒▒▒▒▒▒▒▒▒▒▒▒▄▒▒▒▒▒▒▒▒▒▒▒▒▒▀▄──────
▀█▒▒█▌▒▒█▒▒▐█▒▒▀▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▌─────
▀▌▒▒▒▒▒▀▒▀▒▒▒▒▒▀▀▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▐───▄▄
▐▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▌▄█▒█
▐▒▒▒▒mimikittenz-1.0-alpha▒▒▒▒▒▒▒▒▒▐▒█▀─
▐▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▐▀───
▐▒▒▒▒▒▒CAN I HAZ WAM?▒▒▒▒▒▒▒▒▒▒▒▒▌────
─▌▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▐─────
─▐▒▒▒jamieson@dringensec.com▒▒▒▒▌─────
──▌▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▐──────
──▐▄▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▄▌──────
────▀▄▄▀▀▀▀▄▄▀▀▀▀▀▀▄▄▀▀▀▀▀▀▄▄▀────────

PatternName PatternMatch
----------- ------------
Heist       login_username=admin@support.htb&login_password=4dD!5}x/re8]FBuZ&login=
```

## Scheduled Tasks

Enumerate scheduled tasks with `schtasks`:

Options:

* /query: Peform the query
* /fo LIST: Use a list format
* /v: Verbose

We're interested in this:

* Next Run Time
* Last Run Time
* Task To Run
* Schedule Type
* Start Time
* Start Date

```
C:\Users\low>schtasks /query /fo LIST /v

Folder: \
HostName:                             DESKTOP-L3RJJKV
TaskName:                             \OneDrive Reporting Task-S-1-5-21-3704913888-2176354890-1457497240-1001
Next Run Time:                        1/12/2022 6:04:35 PM
Status:                               Ready
Logon Mode:                           Interactive only
Last Run Time:                        1/6/2022 8:39:41 AM
Last Result:                          -2147023829
Author:                               Microsoft Corporation
Task To Run:                          %localappdata%\Microsoft\OneDrive\OneDriveStandaloneUpdater.exe /reporting
Start In:                             N/A
Comment:                              N/A
Scheduled Task State:                 Enabled
Idle Time:                            Disabled
Power Management:                     Stop On Battery Mode
Run As User:                          low
Delete Task If Not Rescheduled:       Disabled
Stop Task If Runs X Hours and X Mins: 02:00:00
Schedule:                             Scheduling data is not available in this format.
Schedule Type:                        One Time Only, Hourly
Start Time:                           6:04:35 PM
Start Date:                           12/28/2021
End Date:                             N/A
Days:                                 N/A
Months:                               N/A
Repeat: Every:                        24 Hour(s), 0 Minute(s)
Repeat: Until: Time:                  None
Repeat: Until: Duration:              Disabled
Repeat: Stop If Still Running:        Disabled
```

## Installed Applications and Patch Levels



Enumerate applications and their version including the vendor:

```
C:\Users\low>wmic product get name, version, vendor
Name                                                            Vendor                 Version
Microsoft Visual C++ 2019 X86 Additional Runtime - 14.28.29913  Microsoft Corporation  14.28.29913
Microsoft Visual C++ 2019 X64 Additional Runtime - 14.28.29913  Microsoft Corporation  14.28.29913
VMware Tools                                                    VMware, Inc.           11.3.5.18557794
Microsoft Visual C++ 2019 X64 Minimum Runtime - 14.28.29913     Microsoft Corporation  14.28.29913
Microsoft Update Health Tools                                   Microsoft Corporation  2.87.0.0
Microsoft Visual C++ 2019 X86 Minimum Runtime - 14.28.29913     Microsoft Corporation  14.28.29913
```

> Note: This only list applications that are installed by windows intaller.

## Readable/Writable Files and Directories



Enumerate insecure file permissions, in this case we're looking for writable permissions in the Everyone group:

```
accesschk.exe -uws "Everyone" "C:\Program Files"
```

Options:

* u: Supress errors (filter errors)
* w: Write permissions
* s: Recursive search

Alternatively, we can use powershell to do the same thing:

```powershell
Get-ChildItem "C:\Program Files" -Recurse | Get-ACL | ?{$_.AccessToString -match "Everyone\sAllow\s\sModify"}
```

PowerShell cmdlets:

* Get-ACL: Retrive all the permissions for a file or directory
* Get-ChildItem: List the files and directories under `C:\Program Files` recursively with -Recurse

Properties:

* AccessToString: Access strings of the output.

Flag:

* \-match: Filter the strings "Everyone, Allow, Modify"

## Unmounted Disks

Interesting information may be stored in unmounted disks.

Enumerate all drives that are on the system:

```
PS C:\Users\low> mountvol
Creates, deletes, or lists a volume mount point.

MOUNTVOL [drive:]path VolumeName
MOUNTVOL [drive:]path /D
MOUNTVOL [drive:]path /L
MOUNTVOL [drive:]path /P
MOUNTVOL /R
MOUNTVOL /N
MOUNTVOL /E
MOUNTVOL drive: /S

    path        Specifies the existing NTFS directory where the mount
                point will reside.
    VolumeName  Specifies the volume name that is the target of the mount
                point.
    /D          Removes the volume mount point from the specified directory.
    /L          Lists the mounted volume name for the specified directory.
    /P          Removes the volume mount point from the specified directory,
                dismounts the volume, and makes the volume not mountable.
                You can make the volume mountable again by creating a volume
                mount point.
    /R          Removes volume mount point directories and registry settings
                for volumes that are no longer in the system.
    /N          Disables automatic mounting of new volumes.
    /E          Re-enables automatic mounting of new volumes.
    /S          Mount the EFI System Partition on the given drive.

Possible values for VolumeName along with current mount points are:

    \\?\Volume{b6b006bf-7815-47fe-9c48-4456a4a22137}\
        C:\

    \\?\Volume{c0231aa6-87c7-4eed-baff-75563ddfff0c}\
        *** NO MOUNT POINTS ***

    \\?\Volume{57029791-848c-4a1c-aed3-43146b6776b5}\
        *** NO MOUNT POINTS ***

    \\?\Volume{f460c6e9-604f-11ec-901c-806e6f6e6963}\
        D:\
```

The output above reveals this current mount points:

```
    \\?\Volume{b6b006bf-7815-47fe-9c48-4456a4a22137}\
        C:\

    \\?\Volume{c0231aa6-87c7-4eed-baff-75563ddfff0c}\
        *** NO MOUNT POINTS ***

    \\?\Volume{57029791-848c-4a1c-aed3-43146b6776b5}\
        *** NO MOUNT POINTS ***

    \\?\Volume{f460c6e9-604f-11ec-901c-806e6f6e6963}\
        D:\
```

## Device Drivers

\


Enumerate device drivers with powershell:

```powershell
PS C:\Users\low> driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object 'Display Name', 'Start Mode', 'Path'

Display Name                                        Start Mode Path
------------                                        ---------- ----
1394 OHCI Compliant Host Controller                 Manual     C:\Windows\system32\drivers\1394ohci.sys
3ware                                               Manual     C:\Windows\system32\drivers\3ware.sys
Microsoft ACPI Driver                               Boot       C:\Windows\system32\drivers\ACPI.sys
ACPI Devices driver                                 Manual     C:\Windows\system32\drivers\AcpiDev.sys
Microsoft ACPIEx Driver                             Boot       C:\Windows\system32\Drivers\acpiex.sys
ACPI Processor Aggregator Driver                    Manual     C:\Windows\system32\drivers\acpipagr.sys
ACPI Power Meter Driver                             Manual     C:\Windows\system32\drivers\acpipmi.sys
ACPI Wake Alarm Driver                              Manual     C:\Windows\system32\drivers\acpitime.sys
Acx01000                                            Manual     C:\Windows\system32\drivers\Acx01000.sys
<...SNIP...>
```

Options:

* /v: Verbose
* /fo: CSV format

cmdlets:

* ConvertFrom-CSV: Receive CSV ouput and convert it
* Select-Object: Filter the objects properties "Display Name, Start Mode, and Path"

Enumerate the device drivers version number with powershell:

```powershell
PS C:\Users\low> Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "*VMware*"}

DeviceName                 DriverVersion Manufacturer
----------                 ------------- ------------
VMware USB Pointing Device 12.5.10.0     VMware, Inc.
VMware SVGA 3D             8.17.3.5      VMware, Inc.
VMware VMCI Bus Device     9.8.16.0      VMware, Inc.
VMware Pointing Device     12.5.10.0     VMware, Inc.
```

cmdlets:

* Win32\_PnPSignedDriver: Signature
* Select-Object: Object properties in this case `DeviceName, DriverVersion, and Manufacturer`
* Where-Object: Filter the object using the property `DeviceName` with the flag `-like`

## AutoElevate Binaries

Enumerate the AlwaysInstallElevated registry key:

```
C:\Users\low>reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
C:\Users\low>reg query HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer
```
