# Inspecting Binaries

## Inspecting Binaries

We can inspect binaries content to gather more information.

### readelf <a href="#readelf" id="readelf"></a>

**readelf** displays information about one or more ELF format object files. The options control what particular information to display.

32-bit and 64-bit ELF files are supported, as are archives containing ELF files.

This program performs a similar function to objdump but it goes into more detail and it exists independently of the BFD library, so if there is a bug in BFD then readelf will not be affected.

```
readelf -a [binary]
```

### ltrace <a href="#ltrace" id="ltrace"></a>

**ltrace** is a program that simply runs the specified command until it exits. It intercepts and records the dynamic library calls which are called by the executed process and the signals which are received by that process. It can also intercept and print the system calls executed by the program.

```
ltrace [options] filename
```

### strace <a href="#strace" id="strace"></a>

**strace** is a useful diagnostic, instructional, and debugging tool. System administrators, diagnosticians, and trouble-shooters will find it invaluable for solving problems with programs for which the source is not readily available since they do not need to be recompiled in order to trace them. It helps you find information about the system and its system calls by tracing even ordinary programs. Programmers will find that since system calls and signals are events that happen at the user/kernel interface, a close examination of this boundary is very useful for bug isolation, sanity checking and attempting to capture race conditions.

```
strace [options] filename
```

### objdump <a href="#objdump" id="objdump"></a>

&#x20;**objdump** displays information about one or more object files. The options control what particular information to display. This information is mostly useful to programmers who are working on the compilation tools, as opposed to programmers who just want their program to compile and work.

```
objdump [options] filename
```

### strings <a href="#strings" id="strings"></a>

**strings** print the strings of printable characters in files.

```
strings filename
```

## References

{% embed url="https://linux.die.net/man/1/strings" %}

{% embed url="https://linux.die.net/man/1/objdump" %}

{% embed url="https://man7.org/linux/man-pages/man1/ltrace.1.html" %}

{% embed url="https://man7.org/linux/man-pages/man1/readelf.1.html" %}

