# Cracking Protections

## Protecting against Crackers

Just because something is "protected" doesn't mean that's is not vulnerable or uncrackable, however, learning about protections is good for both teams.

## The Impossible

It is impossible to prevent your software from reverse engineering and from being cracked. All that can be made is to make the process more "difficult" or "challenging".

### Software Protection Tools

There are some software-protection tools vendors like:

{% embed url="https://www.wibu.com" %}

### Realistic Answer

Most strong anti-cracking relies on encryption \(symmetric or public key\). The encryption can be very strong, but unless the key storage/generation is equally strong it can be attacked. Lots of other methods are possible too, even with good encryption, unless you know what you are doing. 

A software-only solution will have to store the key in an accessible place, easily found or vulnerable to a man-in-the-middle attack. The same thing is true with keys stored on a web server. Even with good encryption and secure key storage, unless you can detect debuggers the cracker can just take a **snapshot of memory** and **build an exe** from that. So you need to never completely decrypt in memory at any one time and have some code for **debugger detection**. 

* Obfuscation, dead code, etc, **won't** slow them down for long because they don't crack by starting at the beginning and working through your code. They are far more clever than that. 

Extracted from here:

{% embed url="https://stackoverflow.com/questions/5551016/how-to-make-a-good-anti-crack-protection" %}





