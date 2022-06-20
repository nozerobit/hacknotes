# Cracking AD Hashes

We can find Hashcat hash modes here:

{% embed url="https://hashcat.net/wiki/doku.php?id=example_hashes" %}
Hashcat Example Hashes
{% endembed %}

Cracking NT (NTLM) hashes

```shell
$ hashcat -m 1000 -a 0 hashes.txt [path/to/wordlist.txt] -o cracked.txt
$ john --wordlist=[path/to/wordlist.txt] hashes.txt
```

> Note: It is no longer required to specify the mode. (-m)

Kerberoasting - Crack SPN hashes via. exported `.kirbi` tickets.

* Example Walkthrough: https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/t1208-kerberoasting

```shell
# Kerberoast
$ python3 tgsrepcrack.py rockyou.txt [ticket.kirbi]  # locally crack hashes
PS> Invoke-Kerberoast.ps1                            # crack hashes on target

# John the Ripper
$ python3 kirbi2john.py -o johncrackfile ticket.kirbi  # convert ticket to john file
$ john --wordlist=rockyou.txt johncrackfile
```
