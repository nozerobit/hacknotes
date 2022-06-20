# Encrypted File Systems

## Nix

### LUKS

The **Linux Unified Key Setup** \(**LUKS**\) is a [disk encryption](https://en.wikipedia.org/wiki/Disk_encryption) specification created by Clemens Fruhwirth in 2004 and was originally intended for [Linux](https://en.wikipedia.org/wiki/Linux).

While most [disk encryption software](https://en.wikipedia.org/wiki/Disk_encryption_software) implements different, incompatible, and undocumented formats\[[citation needed](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)\], LUKS implements a platform-independent standard on-disk format for use in various tools. This not only facilitates compatibility and interoperability among different programs, but also assures that they all implement [password management](https://en.wikipedia.org/wiki/Password_management) in a secure and documented manner.[\[1\]](https://en.wikipedia.org/wiki/Linux_Unified_Key_Setup#cite_note-1)

The [reference implementation](https://en.wikipedia.org/wiki/Reference_implementation_%28computing%29) for LUKS operates on Linux and is based on an enhanced version of [cryptsetup](https://en.wikipedia.org/wiki/Cryptsetup), using [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) as the disk encryption backend. Under [Microsoft](https://en.wikipedia.org/wiki/Microsoft) [Windows](https://en.wikipedia.org/wiki/Windows), LUKS-encrypted disks can be used with the now defunct [FreeOTFE](https://en.wikipedia.org/wiki/FreeOTFE) \(formerly DoxBox, LibreCrypt\).

LUKS is designed to conform to the [TKS1](https://en.wikipedia.org/w/index.php?title=TKS1&action=edit&redlink=1) secure key setup scheme.[\[2\]](https://en.wikipedia.org/wiki/Linux_Unified_Key_Setup#cite_note-tks1-2)

Extracted from [Wikipedia](https://en.wikipedia.org/wiki/Linux_Unified_Key_Setup)

## Windows

### EFS

The Encrypted File System, or EFS, provides an additional level of security for files and directories. It provides cryptographic protection of individual files on NTFS file system volumes using a public-key system.

Typically, the access control to file and directory objects provided by the Windows security model is sufficient to protect unauthorized access to sensitive information. However, if a laptop that contains sensitive data is lost or stolen, the security protection of that data may be compromised. Encrypting the files increases security.

To determine whether a file system supports file encryption for files and directories, call the [**GetVolumeInformation**](https://docs.microsoft.com/en-us/windows/desktop/api/FileAPI/nf-fileapi-getvolumeinformationa) function and examine the **FS\_FILE\_ENCRYPTION** bit flag. Note that the following items cannot be encrypted:

* Compressed files
* System files
* System directories
* Root directories
* Transactions

Sparse files can be encrypted.

The information above was extracted from here:

{% embed url="https://docs.microsoft.com/en-us/windows/win32/fileio/file-encryption" %}

### BitLocker

BitLocker Drive Encryption is a data protection feature that integrates with the operating system and addresses the threats of data theft or exposure from lost, stolen, or inappropriately decommissioned computers.

BitLocker provides the most protection when used with a Trusted Platform Module \(TPM\) version 1.2 or later. The TPM is a hardware component installed in many newer computers by the computer manufacturers. It works with BitLocker to help protect user data and to ensure that a computer has not been tampered with while the system was offline.

On computers that do not have a TPM version 1.2 or later, you can still use BitLocker to encrypt the Windows operating system drive. However, this implementation will require the user to insert a USB startup key to start the computer or resume from hibernation. Starting with Windows 8, you can use an operating system volume password to protect the operating system volume on a computer without TPM. Both options do not provide the pre-startup system integrity verification offered by BitLocker with a TPM.

In addition to the TPM, BitLocker offers the option to lock the normal startup process until the user supplies a personal identification number \(PIN\) or inserts a removable device, such as a USB flash drive, that contains a startup key. These additional security measures provide multifactor authentication and assurance that the computer will not start or resume from hibernation until the correct PIN or startup key is presented.

The information above was extracted from here:

{% embed url="https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview" %}



