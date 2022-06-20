# Encryption/Decryption

## Encryption

The process of converting human-readable data \(plaintext\) into unintelligible ciphertext is known as encryption. This data scrambling is the result of an algorithmic operation involving a cryptographic key. Simply put, encryption randomizes your data so that anyone who steals it cannot read it unless they have the key to convert it back to a legible form.

### Asymmetric Encryption

Asymmetric encryption employs a pair of related keys, one public and one private. The public key, which is available to all, is used to encrypt a plaintext message before it is sent. You must have the private key to decrypt and read this message. Although the public and private keys are mathematically related, the private key cannot be derived from the public key.

Because the key's security must be maintained, the private key is only shared with the key's initiator in asymmetric encryption \(also known as public-key cryptography or public key encryption\).

![](https://sectigostore.com/blog/wp-content/uploads/2020/05/symmetric-vs-asymmetric-asymmetric-encryption-example-1024x424.png)

### Symmetric Encryption

The same key is used for both encrypting and decrypting messages in the case of symmetric encryption. The entire mechanism does not scale well because it is dependent on keeping the key a shared secret that is, it must be shared with the recipient in a secure manner so that only they can use it to decrypt the message.

Block ciphers or stream ciphers can be used in symmetric encryption algorithms. A number of bits \(in chunks\) are encrypted as a single unit using block ciphers. AES, for example, employs a 128-bit block size with three key length options: 128, 192, or 256 bits.

Symmetric encryption suffers from key exhaustion issues, and without proper key hierarchy maintenance or effective key rotation, every usage may leak information that an attacker could use to reconstruct the secret key.

