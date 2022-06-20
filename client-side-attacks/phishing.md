# Phishing

## Phishing

Phishing is the act of deceiving people into sharing sensitive information like passwords and credit card numbers or convincing the user to give us a foothold. Victims receive a malicious email \([malspam](https://blog.malwarebytes.com/glossary/malspam/)\) or a text message that imitates \(or “[spoofs](https://blog.malwarebytes.com/cybercrime/2016/06/email-spoofing/)”\) a person or organization they trust, like a coworker, a bank, or a government office. When the victim opens the email or text, they find a scary message meant to overcome their better judgment by filling them with fear. The message demands that the victim go to a website and take immediate action or risk some sort of consequence. 

Reference:

{% embed url="https://www.malwarebytes.com/phishing" %}

### Email Phishing

We can use emails to convince users to do an action for us like clicking on a malicious link that gives executes a reverse shell to gain access to the target system. 

### swaks

Swaks - Swiss Army Knife for SMTP

We can email with [swaks ](https://github.com/jetmore/swaks)like this:

```text
swaks --to $(cat emails.txt | tr '\n' ',' | less) --from test@sneakymailer.htb --header "Subject: test" --body "please click here http://10.10.14.42/" --server 10.10.10.197
```

