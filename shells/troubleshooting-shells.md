# Troubleshooting Shells

## The Problem

Have you ever faced a machine where you have remote command execution but you can't receive a reverse shell?&#x20;

If your answer is Yes then this page is for you.

## Troubleshooting Methodology

### Step 1

Let's startup with the obvious, use a program or language that exist in the target operating system. If the system has a powershell, then try powershell, if the system has bash then try bash. If there are some policies which restricts these, then try others, like Python, C, C++, C# and so on. Just make sure that it can be executed.

### Step 2

Make sure that there are no firewall outbound rules or inbound rules for the port that you'll be connecting on.

### Step 3

Try to listen on ports that are open on the target, for example, if the target has port 80 open and you want to receive a reverse shell try to setup a listener on port 80, instead of a random port.

I have wasted hours setting up listeners on random ports and I just couldn't catch a shell... but when I tried a port that's already open on the machine. For example, you did a port scan and found out that port 22 is open. Well setup a listener on port 22 on your attacker host, and probably you will catch a reverse shell.

### Step 4

Avoid badchars, some vulnerabilites that achieve remote command execution may have bad characters, meaning characters that can interrupt the exploit. Fuzz every character and see which ones are allowed.

### Step 5

Use encodings, like base64 to bypass blacklists or whitelists.&#x20;

### Step 6

If all the previous steps fail, make sure that you have a connection to the machine. Enumerate for more devices or technologies that may be interrupting the exploit.&#x20;



