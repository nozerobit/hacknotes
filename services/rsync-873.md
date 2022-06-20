# Rsync - 873

## Rsync

Rsync is a utility for efficiently transferring and synchronizing files between a computer and an external hard drive and across networked computers by comparing the modification times and sizes of files. It is commonly found on Unix-like operating systems. Rsync is written in C as a single-threaded application. [Wikipedia](https://en.wikipedia.org/wiki/Rsync)

## Enumeration

```bash
rsync rsync://10.10.10.10
```

Let's copy the files to our local machine.

```bash
rsync -avz rsync://10.10.10.10/directory_name directory_name
```

