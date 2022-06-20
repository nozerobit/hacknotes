# Linux Enumeration

## Users and Groups

See the current user:

```shell
low@ubuntu:~$ whoami
low
low@ubuntu:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),132(lxd),133(sambashare)
```

Enumerate all the users in the system:

```shell
low@ubuntu:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:114::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:115::/nonexistent:/usr/sbin/nologin
avahi-autoipd:x:109:116:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/usr/sbin/nologin
usbmux:x:110:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
rtkit:x:111:117:RealtimeKit,,,:/proc:/usr/sbin/nologin
dnsmasq:x:112:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
cups-pk-helper:x:113:120:user for cups-pk-helper service,,,:/home/cups-pk-helper:/usr/sbin/nologin
speech-dispatcher:x:114:29:Speech Dispatcher,,,:/run/speech-dispatcher:/bin/false
avahi:x:115:121:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/usr/sbin/nologin
kernoops:x:116:65534:Kernel Oops Tracking Daemon,,,:/:/usr/sbin/nologin
saned:x:117:123::/var/lib/saned:/usr/sbin/nologin
nm-openvpn:x:118:124:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
hplip:x:119:7:HPLIP system user,,,:/run/hplip:/bin/false
whoopsie:x:120:125::/nonexistent:/bin/false
colord:x:121:126:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin
geoclue:x:122:127::/var/lib/geoclue:/usr/sbin/nologin
pulse:x:123:128:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
gnome-initial-setup:x:124:65534::/run/gnome-initial-setup/:/bin/false
gdm:x:125:130:Gnome Display Manager:/var/lib/gdm3:/bin/false
sssd:x:126:131:SSSD system user,,,:/var/lib/sss:/usr/sbin/nologin
low:x:1000:1000:low,,,:/home/low:/bin/bash
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
sshd:x:127:65534::/run/sshd:/usr/sbin/nologin
```

Enumerate groups:

```bash
groups
```

## Hostname

Enumerate the hostname:

```shell
low@ubuntu:~$ hostname
ubuntu
```

## Operating System Version and Architecture

We can enumerate the system:

```shell
low@ubuntu:~$ cat /etc/issue
Ubuntu 20.04.3 LTS \n \l

low@ubuntu:~$ cat /etc/*-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.3 LTS"
NAME="Ubuntu"
VERSION="20.04.3 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.3 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal

low@ubuntu:~$ uname -a
Linux ubuntu 5.11.0-43-generic #47~20.04.2-Ubuntu SMP Mon Dec 13 11:06:56 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
```

## Running Processes and Services

Enumerate all processes in unix based systems:

```shell
low@ubuntu:~$ ps axu
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.3 168844 12788 ?        Ss   Jan11   0:06 /sbin/init auto noprompt
root           2  0.0  0.0      0     0 ?        S    Jan11   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   Jan11   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   Jan11   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   Jan11   0:00 [kworker/0:0H-events_highpri]
root           9  0.0  0.0      0     0 ?        I<   Jan11   0:00 [mm_percpu_wq]
root          10  0.0  0.0      0     0 ?        S    Jan11   0:00 [rcu_tasks_rude_]
root          11  0.0  0.0      0     0 ?        S    Jan11   0:00 [rcu_tasks_trace]
<...SNIP...>
```

## Network Information

Enumerate all the network interfaces:

```shell
low@ubuntu:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:35:5b:cd brd ff:ff:ff:ff:ff:ff
    altname enp2s1
    inet 192.168.254.132/24 brd 192.168.254.255 scope global dynamic noprefixroute ens33
       valid_lft 1678sec preferred_lft 1678sec
    inet6 fe80::8450:b1b6:be0a:a735/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

Enumerate routing tables:

```shell
/sbin/route
```

Enumerate the active network connections:

```shell
ss -nap
Netid               State                Recv-Q               Send-Q                                                            Local Address:Port                                         Peer Address:Port                 Process                                                                                                                                                                                                                      
nl                  UNCONN               0                    0                                                                             0:1656                                                     *                                                                                                                                                                                                                                                  
nl                  UNCONN               0                    0                                                                             0:1669                                                     *                                                                                                                                                                                                                                                  
nl                  UNCONN               0                    0                                                                             0:753                                                      *                                                                                                                                                                                                                               
```

## Firewalls Status and Rules

In unix based systems we need root permissions to enumerate the firewall, however with iptables we can create dumps or saved the configuration of the firewall in a file so we could try to find this files (if any):

```shell
grep -Hs iptables /etc/*
```

Then if a file is found we could try to read it.

## Cron Jobs / Scheduled Tasks

Enumerate the crontabs:

```shell
low@ubuntu:~$ ls -la /etc/cron*
-rw-r--r-- 1 root root 1042 Feb 13  2020 /etc/crontab

/etc/cron.d:
total 32
drwxr-xr-x   2 root root  4096 Dec 20 19:55 .
drwxr-xr-x 128 root root 12288 Jan  7 07:58 ..
-rw-r--r--   1 root root   285 Jul 16  2019 anacron
-rw-r--r--   1 root root   201 Feb 14  2020 e2scrub_all
-rw-r--r--   1 root root   102 Feb 13  2020 .placeholder
-rw-r--r--   1 root root   190 Dec 20 19:54 popularity-contest

/etc/cron.daily:
total 64
drwxr-xr-x   2 root root  4096 Dec 20 20:09 .
drwxr-xr-x 128 root root 12288 Jan  7 07:58 ..
-rwxr-xr-x   1 root root   311 Jul 16  2019 0anacron
-rwxr-xr-x   1 root root   376 Dec  4  2019 apport
-rwxr-xr-x   1 root root  1478 Apr  9  2020 apt-compat
-rwxr-xr-x   1 root root   355 Dec 29  2017 bsdmainutils
-rwxr-xr-x   1 root root   384 Nov 19  2019 cracklib-runtime
-rwxr-xr-x   1 root root  1187 Sep  5  2019 dpkg
-rwxr-xr-x   1 root root   377 Jan 21  2019 logrotate
-rwxr-xr-x   1 root root  1123 Feb 25  2020 man-db
-rw-r--r--   1 root root   102 Feb 13  2020 .placeholder
-rwxr-xr-x   1 root root  4574 Jul 18  2019 popularity-contest
-rwxr-xr-x   1 root root   214 May 14  2021 update-notifier-common

/etc/cron.hourly:
total 20
drwxr-xr-x   2 root root  4096 Aug 19 06:30 .
drwxr-xr-x 128 root root 12288 Jan  7 07:58 ..
-rw-r--r--   1 root root   102 Feb 13  2020 .placeholder

/etc/cron.monthly:
total 24
drwxr-xr-x   2 root root  4096 Aug 19 06:40 .
drwxr-xr-x 128 root root 12288 Jan  7 07:58 ..
-rwxr-xr-x   1 root root   313 Jul 16  2019 0anacron
-rw-r--r--   1 root root   102 Feb 13  2020 .placeholder

/etc/cron.weekly:
total 32
drwxr-xr-x   2 root root  4096 Dec 20 20:09 .
drwxr-xr-x 128 root root 12288 Jan  7 07:58 ..
-rwxr-xr-x   1 root root   312 Jul 16  2019 0anacron
-rwxr-xr-x   1 root root   813 Feb 25  2020 man-db
-rw-r--r--   1 root root   102 Feb 13  2020 .placeholder
-rwxr-xr-x   1 root root   403 Aug  5 10:01 update-notifier-common

low@ubuntu:~$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

## Installed Applications and Patch Levels

Linux based systems use various package managers:

```shell
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                                       Version                               Architecture Description
+++-==========================================-=====================================-============-===============================================================================
ii  accountsservice                            0.6.55-0ubuntu12~20.04.5              amd64        query and manipulate user account information
ii  acl                                        2.2.53-6                              amd64        access control list - utilities
ii  acpi-support                               0.143                                 amd64        scripts for handling many ACPI events
ii  acpid                                      1:2.0.32-1ubuntu1                     amd64        Advanced Configuration and Power Interface event daemon
```

## Readable/Writable Files and Directories

Find every directory writable by our current user:

```shell
low@ubuntu:~$ find / -writable -type d 2>/dev/null
/var/lib/BrlAPI
/var/tmp
/var/metrics
/var/crash
/proc/21547/task/21547/fd
/proc/21547/fd
/proc/21547/map_files
/dev/mqueue
<...SNIP...>
```

Find every file writable by out current user:

```shell
low@ubuntu:~$ find / -writable -type f 2>/dev/null | grep -vE '.cache|/run|/sys|/proc'
/home/low/.sudo_as_admin_successful
/home/low/.bash_history
/home/low/.viminfo
/home/low/.modules.order.cmd
/home/low/.profile
/home/low/.bashrc
<...SNIP...>
```

## Unmounted Disks

We can use the mount command to see mounted file systems:

```shell
low@ubuntu:~$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,noexec,relatime,size=1962364k,nr_inodes=490591,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=399020k,mode=755,inode64)
/dev/sda5 on / type ext4 (rw,relatime,errors=remount-ro)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,inode64)
tmpfs on /run/lock type tmpfs (rw,nosuid,nodev,noexec,relatime,size=5120k,inode64)
tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,mode=755,inode64)
cgroup2 on /sys/fs/cgroup/unified type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate)
cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,name=systemd)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
none on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
cgroup on /sys/fs/cgroup/net_cls,net_prio type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls,net_prio)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpu,cpuacct)
cgroup on /sys/fs/cgroup/rdma type cgroup (rw,nosuid,nodev,noexec,relatime,rdma)
cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,pids)
cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)
cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=28,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=27295)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,pagesize=2M)
mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime)
tracefs on /sys/kernel/tracing type tracefs (rw,nosuid,nodev,noexec,relatime)
debugfs on /sys/kernel/debug type debugfs (rw,nosuid,nodev,noexec,relatime)
/var/lib/snapd/snaps/core18_2128.snap on /snap/core18/2128 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/gnome-3-34-1804_72.snap on /snap/gnome-3-34-1804/72 type squashfs (ro,nodev,relatime,x-gdu.hide)
fusectl on /sys/fs/fuse/connections type fusectl (rw,nosuid,nodev,noexec,relatime)
/var/lib/snapd/snaps/gtk-common-themes_1515.snap on /snap/gtk-common-themes/1515 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/snap-store_547.snap on /snap/snap-store/547 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/snapd_12704.snap on /snap/snapd/12704 type squashfs (ro,nodev,relatime,x-gdu.hide)
configfs on /sys/kernel/config type configfs (rw,nosuid,nodev,noexec,relatime)
vmware-vmblock on /run/vmblock-fuse type fuse.vmware-vmblock (rw,relatime,user_id=0,group_id=0,default_permissions,allow_other)
/dev/sda1 on /boot/efi type vfat (rw,relatime,fmask=0077,dmask=0077,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro)
tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nodev,relatime,size=399020k,mode=700,uid=1000,gid=1000,inode64)
gvfsd-fuse on /run/user/1000/gvfs type fuse.gvfsd-fuse (rw,nosuid,nodev,relatime,user_id=1000,group_id=1000)
/var/lib/snapd/snaps/snapd_14295.snap on /snap/snapd/14295 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/bare_5.snap on /snap/bare/5 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/core18_2253.snap on /snap/core18/2253 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/core20_1270.snap on /snap/core20/1270 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/gtk-common-themes_1519.snap on /snap/gtk-common-themes/1519 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/snap-store_558.snap on /snap/snap-store/558 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/gnome-3-34-1804_77.snap on /snap/gnome-3-34-1804/77 type squashfs (ro,nodev,relatime,x-gdu.hide)
/var/lib/snapd/snaps/gnome-3-38-2004_87.snap on /snap/gnome-3-38-2004/87 type squashfs (ro,nodev,relatime,x-gdu.hide)
tmpfs on /run/snapd/ns type tmpfs (rw,nosuid,nodev,noexec,relatime,size=399020k,mode=755,inode64)
nsfs on /run/snapd/ns/snap-store.mnt type nsfs (rw)
binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,nosuid,nodev,noexec,relatime)
```

The `/etc/fstab` file contains information about all the drives mounted at boot time:

```shell
low@ubuntu:~$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda5 during installation
UUID=8d394a4d-42aa-40dc-9f6a-5510377bd8a6 /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda1 during installation
UUID=ED7E-8146  /boot/efi       vfat    umask=0077      0       1
/swapfile                                 none            swap    sw              0       0
/dev/fd0        /media/floppy0  auto    rw,user,noauto,exec,utf8 0       0
```

We can enumerate all the available disk with `lsblk`:

```shell
low@ubuntu:~$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
fd0      2:0    1     4K  0 disk 
loop0    7:0    0   219M  1 loop /snap/gnome-3-34-1804/72
loop1    7:1    0  55.4M  1 loop /snap/core18/2128
loop2    7:2    0  65.1M  1 loop /snap/gtk-common-themes/1515
loop3    7:3    0    51M  1 loop /snap/snap-store/547
loop4    7:4    0  32.3M  1 loop /snap/snapd/12704
loop5    7:5    0  43.3M  1 loop /snap/snapd/14295
loop6    7:6    0     4K  1 loop /snap/bare/5
loop7    7:7    0  55.5M  1 loop /snap/core18/2253
loop8    7:8    0  61.9M  1 loop /snap/core20/1270
loop9    7:9    0  65.2M  1 loop /snap/gtk-common-themes/1519
loop10   7:10   0  54.2M  1 loop /snap/snap-store/558
loop11   7:11   0   219M  1 loop /snap/gnome-3-34-1804/77
loop12   7:12   0 247.9M  1 loop /snap/gnome-3-38-2004/87
sda      8:0    0    20G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0     1K  0 part 
└─sda5   8:5    0  19.5G  0 part /
sr0     11:0    1  1024M  0 rom  
sr1     11:1    1  1024M  0 rom  
```

## Drivers and Kernel Modules

Enumerate the loaded kernel modules:

```shell
low@ubuntu:~$ lsmod
Module                  Size  Used by
binfmt_misc            24576  1
xsk_diag               16384  0
vsock_diag             16384  0
tcp_diag               16384  0
udp_diag               16384  0
raw_diag               16384  0
inet_diag              24576  3 tcp_diag,raw_diag,udp_diag
unix_diag              16384  0
af_packet_diag         16384  0
netlink_diag           16384  0
btrfs                1327104  0
blake2b_generic        20480  0
xor                    24576  1 btrfs
raid6_pq              114688  1 btrfs
ufs                    81920  0
qnx4                   16384  0
<...SNIP...>
```

Enumerate the module information:

```shell
low@ubuntu:~$ modinfo usbhid
filename:       /lib/modules/5.11.0-43-generic/kernel/drivers/hid/usbhid/usbhid.ko
license:        GPL
description:    USB HID core driver
author:         Jiri Kosina
author:         Vojtech Pavlik
author:         Andreas Gal
srcversion:     C014756BC9596EBE3E5E506
alias:          usb:v*p*d*dc*dsc*dp*ic03isc*ip*in*
depends:        hid
retpoline:      Y
intree:         Y
name:           usbhid
vermagic:       5.11.0-43-generic SMP mod_unload modversions 
sig_id:         PKCS#7
signer:         Build time autogenerated kernel key
sig_key:        22:E2:4E:92:8E:E0:C9:46:79:36:5C:5B:EA:55:E3:2C:08:50:11:63
sig_hashalgo:   sha512
signature:      B4:C2:F1:87:C4:8D:B6:17:06:36:BE:5F:7D:C1:75:5F:D6:F2:76
```

In the case that we can't use the relative path, we could try using the absolute path of the modinfo command.

## SUID Binaries

Enumerate SUID bit files:

```shell
low@ubuntu:~$ find / -perm -u=s -type f 2>/dev/null | grep -v snap
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/fusermount
/usr/bin/umount
/usr/bin/chfn
/usr/bin/vmware-user-suid-wrapper
/usr/bin/gpasswd
/usr/bin/pkexec
/usr/bin/su
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/eject/dmcrypt-get-device
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/xorg/Xorg.wrap
/usr/sbin/pppd
```

## Sockets

## Containers

