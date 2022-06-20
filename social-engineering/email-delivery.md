# Email Delivery

## SPF - Sender Policy Framework

Usually when we connect to a mail server and send the MAIL FROM command, we're basically claiming the message comes from the address we provide. However, SMTP does NOT verify this claim.

This issue was fixed with the Sender Policy Framework (SPF), which is a standard to verify this claim. However, this can still be abused because it doesn't verify the message content.

We can enumerate the SPF record of a particular domain with the following:

```bash
dig +short TXT domain.local
```

## DKIM - DomainKeys Identified Mail

DKIM does have a message signing process known as a DKIM-signature header. This can be used to confirm that the message originated from a particular server. Usually, the verification process is performed by a server, through a DNS service querying the domain's public key; which is used to identify if the message originated from that domain.&#x20;

We can enumerate the DKIM record with the following:

```bash
dig dkim._domainkey.domain.local TXT
```

## DMARC - Domain-Based Message Authentication, Reporting, and Conformance

Is a standard that allows a domain owner to perform the following tasks:

* Announce or report the usage of DKIM and SPF
* Alert other mail servers about failures or suspicious activity

We can enumerate DMARC with the following:

```bash
dig +short TXT _dmarc.domain.local
```



