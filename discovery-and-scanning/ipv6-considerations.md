# IPv6 Considerations

## IPv6

This page is just a reminder for when you forget some IPv6 concepts.

### IPv6 Reminders



| Table 1. Types of IPv6 Addresses |                       |               |
| -------------------------------- | --------------------- | ------------- |
| Address type                     | Binary prefix         | IPv6 notation |
| Unspecified                      | 00 . . . 0 (128 bits) | ::/128        |
| Loopback                         | 00 . . . 1 (128 bits) | ::1/128       |
| Multicast                        | 11111111              | FF00::/8      |
| Link-local unicast               | 1111111010            | FE80::/10     |
| Site-local unicast               | 1111111011            | FEC0::/10     |
| Global unicast                   | (everything else)     |               |

Three categories of IP addresses are supported in IPv6:

**Unicast**: An identifier for a single interface. A packet sent to a unicast address is delivered to the interface identified by that address. It can be link-local scope, site-local scope, or global scope.

**Multicast**: An identifier for a group of interfaces (typically belonging to different nodes). A packet sent to a multicast address is delivered to all interfaces identified by that address.

**Anycast**: An identifier for a group of interfaces (typically belonging to different nodes). A packet sent to an anycast address is delivered to the closest member of a group, according to the routing protocols' measure of distance. Anycast addresses are taken from the unicast address spaces (of any scope) and are not syntactically distinguishable from unicast addresses. Anycast is described as a cross between unicast and multicast. Like multicast, multiple nodes may be listening on an anycast address. Like unicast, a packet sent to an anycast address will be delivered to one (and only one) of those nodes. The exact node to which it is delivered is based on the IP routing tables in the network.

There are no broadcast addresses in IPv6. Multicast addresses have superseded this function.

**Link-Local Address** – A special address used to communicate within the local link of an interface (i.e. anyone on the link as host or router)&#x20;

– The address in the packet destination would never pass through a router (local scope)

&#x20;– Mandatory address - automatically assigned as soon as IPv6 is enabled&#x20;

– FE80::/10

**Site-Local Address** – Addresses similar to the RFC 1918 / private address like in IPv4&#x20;

– FEC0::/10 • This address type is now deprecated by RFC 3879 because of a lack of uniqueness

– Ambiguity of addresses&#x20;

– Fuzzy definition of “sites”&#x20;

**Unique Local IPv6 Unicast Address**&#x20;

– Addresses similar to the RFC 1918 (private address) in IPv4&#x20;

– Ensures uniqueness&#x20;

– A part of the prefix (40 bits) are generated using a pseudo-random algorithm and it's improbable that two generated ones are equal&#x20;

– FC00::/7

**IPv6 Global Unicast Address**&#x20;

– Global Unicast Range: 0010 2000::/3 0011 3FFF:FFF:…:FFFF/3&#x20;

– All five RIRs are given a /12 from the /3 to further distribute within the RIR region&#x20;

* APNIC 2400:0000::/12&#x20;
* ARIN 2600:0000::/12&#x20;
* AfriNIC 2C00:0000::/12&#x20;
* LACNIC 2800:0000::/12&#x20;
* Ripe NCC 2A00:0000::/12

**6to4 Addresses**&#x20;

– 2002::/16&#x20;

– Designed for a special tunneling mechanism \[RFC 3056] to connect IPv6 Domains via IPv4 Clouds&#x20;

– Automatic tunnel transition Mechanisms for IPv6 Hosts and Routers&#x20;

– Need 6to4 relay routers in ISP network

Two address ranges are reserved for examples and documentation purposes by RFC 3849&#x20;

– For example 3FFF:FFFF::/32&#x20;

– For documentation 2001:0DB8::/32

## References

{% embed url="https://www.ibm.com/docs/en/zvm/7.1?topic=addressing-types-categories-ipv6-addresses" %}



