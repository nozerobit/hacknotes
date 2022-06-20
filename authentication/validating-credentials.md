# Validating Credentials

## Kerberos Users Validation

Validate if this users exist in the domain:

```
./kerbrute userenum --dc IP -d domain.local users.txt
```

## SMB Credentials Validation

```
crackmapexec smb 192.168.89.122 -u user -p password
```

## WinRM Credentials Validation

```
crackmapexec winrm 192.168.89.122 -u user -p password
```

## LDAP Credentials Validation

```
crackmapexec ldap <IP> -d domain.local -u username -p 'password' --kdcHost <IP>
```

## SSH Credentials Validation

```
crackmapexec ssh 192.168.89.122 -u user -p password
```
