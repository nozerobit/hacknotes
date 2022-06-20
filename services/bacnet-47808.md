# BACNet - 47808

**BACnet** is a [communications protocol](https://en.wikipedia.org/wiki/Communications_protocol) for Building Automation and Control \(BAC\) networks that leverage the [ASHRAE](https://en.wikipedia.org/wiki/ASHRAE), [ANSI](https://en.wikipedia.org/wiki/ANSI), and [ISO](https://en.wikipedia.org/wiki/International_Organization_for_Standardization) 16484-5 standard[\[1\]](https://en.wikipedia.org/wiki/BACnet#cite_note-1) protocol.

BACnet was created to facilitate the communication of building automation and control systems, including HVAC, lighting, access control, and fire detection systems, as well as their associated equipment. The BACnet protocol allows computerized building automation devices to communicate with one another, independent of the type of equipment.

**Default port:** 47808

```text
PORT      STATE SERVICE
47808/udp open  BACNet -- Building Automation and Control NetworksEnumerate
```

## Enumeration <a id="enumeration"></a>

### Manual <a id="manual"></a>

```text
pip3 install BAC0
import BAC0bbmdIP = '<IP>:47808'
bbmdTTL = 900bacnet = BAC0.connect(bbmdAddress=bbmdIP, bbmdTTL=bbmdTTL)
bacnet.vendorName.strValue
```

### Automatic <a id="automatic"></a>

Instead of attempting to join a BACnet network as a foreign device, this script simply sends BACnet queries to an IP addressable device:

```text
nmap --script bacnet-info --script-args full=yes -sU -n -sV -p 47808 <IP>
```



