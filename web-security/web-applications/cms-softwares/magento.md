# Magento

## Magento Information

 Magento is an open-source e-commerce platform written in PHP. It uses multiple other PHP frameworks such as Laminas and Symfony. Magento source code is distributed under Open Software License v3.0. Magento was acquired by Adobe Inc in May 2018 for $1.68 billion. [Wikipedia](https://en.wikipedia.org/wiki/Magento)

{% embed url="https://magento.com/" %}

### Enumeration

We scan this using Magescan. Download the phar file for the latest release from here and then scan the box using it.

```bash
wget https://github.com/steverobbins/magescan/releases/download/v1.12.9/magescan.phar
# Scan
php magescan.phar scan:all http:// 10.10.10.140
```

### Admin Panel

![Magento Admin Panel](../../../.gitbook/assets/image%20%2812%29.png)

Default Credentials:

{% embed url="https://magento.stackexchange.com/questions/231135/what-is-the-default-magento-admin-username-and-password" %}

### Symlink

![Template Symlink](../../../.gitbook/assets/image%20%2816%29.png)

### Reverse Shell

![Upload Reverse Shell](../../../.gitbook/assets/image%20%2815%29.png)

![Mouse Hovering on Reverse Shell File](../../../.gitbook/assets/image%20%2814%29.png)

![New Template with Directory Traversal ](../../../.gitbook/assets/image%20%2813%29.png)

![Preview Template to Excute Reverse Shell File](../../../.gitbook/assets/image%20%2819%29.png)

### Exploits

```text
searchsploit magento
```

