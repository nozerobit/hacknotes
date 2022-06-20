# Man Pages

## man

Not only do man pages offer information about user commands, but they also contain documentation on system management commands, programming interfaces, and other topics. The material of the manual is divided into the following sections, which are numbered:

| Section | Contents                                       |
| ------- | ---------------------------------------------- |
| 1       | User Commands                                  |
| 2       | Programming interfaces for kernel system calls |
| 3       | Programming interfaces to the C library        |
| 4       | Special files such as device nodes and drivers |
| 5       | File formats                                   |
| 6       | Games and amusements such as screen-savers     |
| 7       | Miscellaneous                                  |
| 8       | System administration command                  |

Simply use a keyword search to find the proper manual section.

However, we may run a keyword search with man using the -k option, as seen below:

```
man -k '^passwd$'
```

The regular expression is contained by a caret (^) and a dollar sign ($) in the above command to match the entire line and avoid sub-string matches. By referring to the right part, we can now look at the specific `passwd` manual page we're looking for:

```
man <section_number> passwd
man 5 passwd
```

## apropos

Apropos perform the same function as `man -k` as we can see with:

```
apropos partition
```
