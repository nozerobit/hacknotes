# Apache JServ Protocol (AJP) - 8009

_Ajp13 protocol is packet-oriented TCP protocol, by default this service runs on port 8009. AJP13 protocol is a binary format, which is intended for better performance over the HTTP protocol running over TCP port 8080_.

You'll propably see this running alongside with Apache Tomcat.

### GhostCat - **CVE-2020-10487**

Researching exploits for this service we can see a vulnerability known as Ghostcat. This exploit (**CVE-2020-10487**) allows us to read local files in the Tomcat web directory and even configuration files. Below is a PoC for this on Github.

{% embed url="https://github.com/00theway/Ghostcat-CNVD-2020-10487" %}
