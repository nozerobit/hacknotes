# RFID Security

As with any other wireless or wired communications technology, security is a big concern with RFID. Fortunately, due to the low ranges of RFID, some forms of hacking are not viable, but this does not negate the importance of security. If there is a chance for hackers to profit, they will take advantage of the situation. Furthermore, problems of privacy must be considered, especially given the increased awareness of privacy in all of its forms.

## RFID Tag Cloning

A circuit and an antenna are the only two (2) simple components that make up the RFID tag. Both of these objects are useless unless they are powered. An electrical field is formed when the circuit comes into contact with radio waves at low, high, and ultra-high frequencies. This field provides power to the circuit, allowing it to send data to a nearby source. 

RFID readers, such as room locks, security gates, and stock PDAs, provide power to the chip they are scanning and then use the data for whatever reason is required.

There are two (2) main approaches to perfrom tag clones:

- Rolling code approach: The RFID security employs a scheme in which the RFID tag's identifier changes after each read action. Any observable reactions become less valuable as a result of this. For the identifier to change, both the RFID reader and the RFID tag must use the same algorithm. Multiple readers must be linked in order for tracking to be possible.

- Challenge response authentications: Cryptographic principles are used in these systems. The reader sends an inquiry to the tag, which receives a response, but the system cannot be hacked since secret tag information is never communicated across the RFID reader-tag interface. The outputs of internal cryptographic methods are delivered to both the reader and the tag, together with the correct answers required for a successful information exchange. Encrypting data to transfer over a conventional radio link is essentially the same approach.

## RFID Privacy

RFID tags have unique identifiers that can be used to profile and identify consumer and individual trends. RFID tags on persons can be tracked using stealth readers; RFID tags usually stay active after they've been purchased, thus it's feasible to use them illegally when wearing a clothing, for example.

- Hidden tags could be placed on or within an item to allow for stealth tracking. A variety of ways can be used to help solve these problems:

- Blocker tags: These tags trick unauthorized readers into thinking there are a lot of tags in the area, restricting access to any tags that may be on the person.

Kill switches: Tags can be disabled when consumer items are purchased or if an RFID tag has to be deactivated for any reason. Although not all contemporary RFID tags include this feature, many do. It also doesn't prevent the placement and usage of illegal tags for tracking purposes.
