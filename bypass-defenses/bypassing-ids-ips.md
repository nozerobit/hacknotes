# Bypassing IDS/IPS

There are plenty of techniques that are used to bypass IDS and IPS rules.

## TTL Manipulation

We can manipulate TTL values to bypass IDS/IPS devices.

We can attempt to send a few packets with a TTL that is long enough to reach the IDS/IPS device but not long enough to reach the destination system. Then send more packets with the same sequences as the others so that the IPS/IDS thinks they're duplicates and doesn't check them, but they're actually carrying malicious content.

## Signatures Evasion

We could add data to the packets so the IPS/IDS signature is avoided.

## Fragmented Packets

If we fragment the packets and the IDS/IPS device doesn't have a functionality to reassemble fragmented packets; we could effectively bypass it's security and reach the destination host.

## Insertion

We can 'confuse' the IDS by sending invalid packets. We could craft malformed packets in a way that the targeted systems can interpret the payload but the IDS is unable to recognize the payload.

## Obfuscation

Obfuscation is the process of making normally understandable text or code difficult to read and understand. This is frequently utilized for concerns of security and privacy. Encoding is a method of transforming ordinary text into a particular format that is primarily used for internet transfers.

Examples:

- Changing strings such as string variables can be effective. 

- We can use encodings such as Base64 to obfuscate the code a little.

## Invalid Packets

Another approach to get around an IDS is to send faulty TCP packets. We could change one of the **six TCP flags** or the **packet checksum**.

## Encryption

We can use encryption to encrypt the code, making it more difficult for IDS/IPS to detect the code.