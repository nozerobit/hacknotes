# MAC to IPv6

## MAC Address to IPv6 Link-Local Address

When you find a MAC address, it’s possible, following a procedure to convert this address to the IPv6 link-local address. A link-local address is an IPv6 unicast address that can be automatically configured on any interface using the link-local prefix FE80::/10 (1111 1110 10) and the interface identifier in the modified EUI-64 format. Link-local addresses are not necessarily bound to the MAC address (configured in a EUI-64 format).

### Offline - Manually Convert MAC to IPv6

In the following example, we will convert the MAC address 11:22:33:44:55:66

1. Convert the first octet (11) from hexadecimal to binary
   * **11**:22:33:44:55:66\

   * 11 -> 0001 0001\

2. Invert the 7th bit (if it’s 0 put 1, if it’s 1 put 0)
   * 0001 00**0**1 -> 0001 00**1**1\

3. Convert the octet back into hexadecimal
   * 0001 -> 1\

   * 0011 -> 3\

   * **0001 0011 -> 13**\

4. Replace the original first octet with the newly converted one
   * **11**:22:33:44:55:66 -> **13**:22:33:44:55:66\

5. Add **ff:fe** to the middle of the new MAC address
   * 13:22:33:**ff:fe**:44:55:66\

6. Add **dead:beef::** to the beginning of the address
   * **dead:beef::**13:22:33:ff:fe:44:55:66\

7. Group everything by 4 hex digits
   * dead:beef::1322:33ff:fe44:5566.

> Note: You may use fe80:: instead of dead:beef::

### Online MAC address to IPv6 Converters

{% embed url="https://www.vultr.com/resources/mac-converter/" %}

{% embed url="https://ben.akrin.com/?p=1347" %}

