# Ciphers and Encodings

## Ciphers & Encodings

This page doesn't have all the ciphers and encodings but it does have the most commonly used ciphers and encodings. 

> Note: All the texts on this page were extracted from their corresponding links.

For more ciphers and encodings visit this page:

{% embed url="https://www.dcode.fr/" %}

## Affine

The Affine cipher is a monoalphabetic substitution cipher, where each letter in the alphabet is mapped to another letter through a simple mathematical formula: \(ax + b\) mod 26. The number 26 represents the length of the alphabet and will be different for different languages. The Affine cipher can be broken using the standard statistical methods for monoalphabetic substitution ciphers.

{% embed url="https://www.dcode.fr/affine-cipher" %}

{% embed url="https://cryptii.com/pipes/affine-cipher" %}

{% embed url="https://www.boxentriq.com/code-breaking/affine-cipher" %}

## Atbash

The Atbash Cipher is a really simple substitution cipher that is sometimes called mirror code. It is believed to be the first cipher ever used, and its use pre-dates Egyptian examples of encryption. To use Atbash, you simply reverse the alphabet, so A encodes to Z, B to Y, and so on.

Atbash is considered a special case of Affine Cipher, a monoalphabetic substitution cipher. Affine is encrypted by converting letters to their numerical equivalent \(A=1, Z=26 etc.\), putting that number through a mathematical formula, and then converting the result into letters. With Atbash, the Affine formula is a = b = \(m − 1\), where m is the length of the alphabet.

{% embed url="https://www.boxentriq.com/code-breaking/atbash-cipher" %}

{% embed url="https://www.dcode.fr/atbash-cipher" %}

## Baconian

The Baconian cipher, or Bacon's cipher, is a method for steganography invented by Francis Bacon in 1605. Each letter is translated into a pattern of five digits or the letters 'A' and 'B'. These are then applied to a carrier message. For instance, they could represent two different typefaces or some other property of the letters.

For example, the letter F would translate to aabab. We could apply it to the word 'horse' by letting a = lowercase and b = uppercase. We then get hoRsE. This way of applying the cipher is very easy to detect, but is a good example. There are countless other possibilities in how to apply it.

{% embed url="https://www.boxentriq.com/code-breaking/baconian-cipher" %}

{% embed url="https://www.dcode.fr/bacon-cipher" %}

## Base32

The **Base32** code is described in RFC 4648 standard. It allows to encode with 32 characters:

ABCDEFGHIJKLMNOPQRSTUVWXYZ234567 and the = symbol optionally used as the final character \(as in [Base64](https://www.dcode.fr/base-64-encoding)\).

The message to be encoded is first treated as a binary string \(according to a predefined encoding such as [ASCII](https://www.dcode.fr/ascii-code) or [Unicode](https://www.dcode.fr/unicode-coding)\).

{% embed url="https://www.dcode.fr/base-32-encoding" %}

{% embed url="https://cryptii.com/" %}

## Base64

Base64 is a worldwide standard encoding to represent binary data in an ASCII string. Each Base64 character represents exactly 6 bits of data. Therefore, four Base64 characters represent three bytes of data. A set of 64 characters are used and they vary slightly between different Base64 formats/implementations.

{% embed url="https://www.boxentriq.com/code-breaking/base64-decoder" %}

{% embed url="https://www.dcode.fr/base-64-encoding" %}

{% embed url="https://cryptii.com/" %}

## Bifid

The Bifid cipher was invented by the French amateur cryptographer Félix Delastelle around 1901. It is a fractionating transposition cipher and was considered a system of importance in cryptology. The Bifid cipher uses a Polybius square to achieve the fractionation. Each character depends on two plaintext characters, so it is a digraphic cipher.

{% embed url="https://www.boxentriq.com/code-breaking/bifid-cipher" %}

{% embed url="https://www.dcode.fr/bifid-cipher" %}

{% embed url="https://cryptii.com/" %}

## Caesar Shift

The Caesar cipher, also known as a shift cipher, Caesar's code, or Caesar shift is one of the oldest and most famous ciphers in history. While being deceptively simple, it has been used historically for important secrets and is still popular among puzzlers.

{% embed url="https://www.boxentriq.com/code-breaking/caesar-cipher" %}

{% embed url="https://www.dcode.fr/caesar-cipher" %}

## Keyed Caesar

 The keyed Caesar cipher is a variant of the [Caesar Cipher](https://www.boxentriq.com/code-breaking/caesar-cipher) that provides increased protection. Instead of having all letters in alphabetical order, it starts with a code word \(the encryption key\). Unused letters are then added after the code word. If the code word is blank, it behaves exactly like an ordinary Caesar Cipher.

{% embed url="https://www.boxentriq.com/code-breaking/keyed-caesar-cipher" %}

## Columnar Transposition

 In a columnar transposition cipher, the message is written in a grid of equal length rows, and then read out column by column. The columns are chosen in a scrambled order, decided by the encryption key. Since transposition ciphers doesn't affect the letter frequencies, it can be detected through [frequency analysis](https://www.boxentriq.com/code-breaking/frequency-analysis). Like other transposition ciphers, it can be attacked by moving letters around and anagramming. Also it can be attacked using brute-force methods if the key isn't long enough.

{% embed url="https://www.boxentriq.com/code-breaking/columnar-transposition-cipher" %}

{% embed url="https://www.dcode.fr/columnar-transposition-cipher" %}

## Double Transposition

 The double columnar transposition cipher is considered one of the most secure ciphers that can be performed by hand. It is equivalent to using two [columnar transposition ciphers](https://www.boxentriq.com/code-breaking/columnar-transposition-cipher), with same or different keys. During World War I and II, it was used by various agents and military forces.

{% embed url="https://www.dcode.fr/double-transposition-cipher" %}

{% embed url="https://www.boxentriq.com/code-breaking/double-transposition-cipher" %}

## Cryptogram Solver

A cryptogram is a short piece of encrypted text using any of the classic ciphers. Usually it is simple enough that it can be solved by hand. The most common types of cryptograms are monoalphabetic substitution ciphers, called Aristocrats if they contain spaces or Patristocrats if they don't. Another common name is cryptoquip.

Note: You can use the tool below to solve monoalphabetic substitution ciphers. There are many other types of cryptograms. This [Cipher Identifier Tool](https://www.boxentriq.com/code-breaking/cipher-identifier) will help you identify and solve other types of cryptograms.

{% embed url="https://www.boxentriq.com/code-breaking/cryptogram" %}

## Gronsfeld

{% embed url="https://www.dcode.fr/gronsfeld-cipher" %}

## Morse Code

{% embed url="https://www.dcode.fr/morse-code" %}

{% embed url="http://rumkin.com/tools/cipher/morse.php" %}

## Letter Numbers

{% embed url="http://rumkin.com/tools/cipher/numbers.php" %}

## One Time Pad

{% embed url="http://rumkin.com/tools/cipher/otp.php" %}

## Playfair

{% embed url="http://rumkin.com/tools/cipher/playfair.php" %}

## Railfence

When you rearrange your text in a "wave" sort of pattern \(down, down, up, up, down, down, etc.\), it is called a railfence. Take the text "WAFFLES FOR BREAKFAST" and arrange them in waves like the diagram below. I substituted \* for spaces just to illustrate that the spaces are not removed.

W   L   F   B   K   T  
 A F E \* O \* R A F S  
  F   S   R   E   A

You leave the spaces in. Next, you squish together the lines, remembering to keep the spaces in. I did not replace spaces with stars since the spaces are clearly shown in the middle line.

WLFBKT  
AFE O RAFS  
FSREA

Then you just combine the lines and get WLFBKTAFE O RAFSFSREA. Or you can use this JavaScript-based tool and speed things up quite a bit.

{% embed url="http://rumkin.com/tools/cipher/railfence.php" %}

## ROT13

Rot13 isn't a very secure algorithm. A becomes N, B becomes O, C changes to P, etc. It is used to obscure spoilers and hints so that the person reading has to do a little work in order to understand the message instead of being able to accidentally read it.

Rot13 is both an encoder and decoder. You can enter plain text or encoded text, and you will be given the other one. Just type either one here and it will be automatically encoded or decoded.

{% embed url="http://rumkin.com/tools/cipher/rot13.php" %}

## Rotate

 This cipher is pretty simple. Basically, you would write all of the letters in a grid, then rotate the grid 90° and read the characters back out. I first heard of this method when [Mike](http://groups.yahoo.com/group/Kryptos/message/4834) posted to the [Kryptos Group](http://groups.yahoo.com/group/kryptos) mailing list. I liked the method and decided to write up a neat little encoder. It was used to decode K3. I can insert the [first half](http://rumkin.com/tools/cipher/rotate.php#) for you, then you just copy the decoded text back into the text area above and change the column width to 8 in order to see the secret message.

{% embed url="http://rumkin.com/tools/cipher/rotate.php" %}

## Skip

Basically, if you are given the encrypted text, you start at a given letter and then count N letters \(wrapping around from the end to the beginning\) forward to the next letter. It can be used for the third part of the [Kryptos](http://google.com/search?q=kryptos) statue. I can also pre-load the [K3](http://rumkin.com/tools/cipher/skip.php#) information for you.

If you do use this for decoding the Kryptos, you will see that you need to just count every 192nd letter. Additionally, I have made 5 characters lowercase: The "s" and the "l" are the first two characters, in case you wanted to count by hand. The "y", "a", and "r" are the three letters that are offset from the rest of the text.

{% embed url="http://rumkin.com/tools/cipher/skip.php" %}

## Substitution

A substitution cipher is a pretty basic type of code. You replace every letter with a drawing, color, picture, number, symbol, or another type of letter. This means, if you have your first "E" encoded as a square, all of your other "E"s in the message will also be squares.

{% embed url="http://rumkin.com/tools/cipher/substitution.php" %}

## Übchi

During World War I, the Germans used a double columnar transposition cipher called Übchi \("ubchi" with umlauts\). For a bit more information about columnar transposition ciphers, see that [cipher's page](http://rumkin.com/tools/cipher/coltrans.php). This method is surprisingly similar to the U.S. Army's [double transposition](http://rumkin.com/tools/cipher/coltrans-double.php), also used during World War I.

{% embed url="http://rumkin.com/tools/cipher/ubchi.php" %}

## Vigenere

A 16th century French diplomat, Blaise de Vigenere, created a very simple cipher that is moderately difficult for any unintended parties to decipher. It is somewhat like a variable [Caesar](http://rumkin.com/tools/cipher/caesar.php) cipher, but the N changed with every letter. You would "encode" your message with a passphrase, and the letters of your passphrase would determine how each letter in the message would be encrypted.

This is the exact opposite of a "Variant Beaufort." To do the variant, just "decode" your plain text to get the cipher text and "encode" the cipher text to get the plain text again.

{% embed url="http://rumkin.com/tools/cipher/vigenere.php" %}

## Keyed Vigenere

Based on the simpler [Vigenere](http://rumkin.com/tools/cipher/vigenere.php) cipher, this uses an alternate tableau. The "Alphabet Key" helps decide the alphabet to use to encrypt and decrypt the message. The "Passphrase" is the code word used to select columns in the tableau. Instead of just using the alphabet from A to Z in order, the alphabet key puts a series of letters first, making the cipher even tougher to break. This style of encryption is also called a Quagmire III.

{% embed url="http://rumkin.com/tools/cipher/vigenere-keyed.php" %}

## Vigenere Autokey

The [Vigenère cipher](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher) is a polyalphabetic substitution cipher that is a natural evolution of the Caesar cipher. The Caesar cipher encrypts by shifting each letter in the plaintext up or down a certain number of places in the alphabet. If the message was right-shifted by 4, each A would become E, and each S would become W.

In the Vigenère cipher, a message is encrypted using a secret key, as well as an encryption table \(called a Vigenere square, Vigenere table, or tabula recta\). The tabula recta typically contain the 26 letters of the Latin alphabet from A to Z along the top of each column and repeated along the left side at the beginning of each row. Each row of the square has the 26 letters of the Latin alphabet, shifted one position to the right in a cyclic way as the rows progress downwards. Once B moves to the front, A moves down to the end. This continues for the entire square.

{% embed url="https://www.boxentriq.com/code-breaking/vigenere-cipher" %}

## XOR

 **XOR** Encryption uses the **XOR** operator \(**Exclusive Or**, symbol: ⊕\) with the plain text and the key as operand \(that should be binary encoded\).

{% embed url="https://www.dcode.fr/xor-cipher" %}







