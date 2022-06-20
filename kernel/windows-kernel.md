# Windows Kernel

Learning the kernel is useful for the following purposes:
- Kernel Development
- Kernel Security Research
- Virtualization Development
- Virtualization Security
- Assist in Anti-Virus Development
- Assist in Anti-Cheat Development
- Assist in Firmware Development
- Assist in Driver Development

These are the prerequisistes that we must know:
- C Language
- Assembly Language
- Windows API

## Drivers

Drivers use the extension `.sys` with the PE format. The entry point of every driver is `DriverEntry`. A driver object represents the driver instance in the kernel and it contains all the information of a driver.  

The registry path is usually located in the location `Registry\Machine\System\CurrentControlSet\Service<i>DriverName`.

Each kernel object has a pre/post callback which corresponds to a different operation.

A driver can communicate with the user-mode application using IOCTL.

IOCTL is used for the following purposes:
- NT Device Name (optional): NT kernel can recognize the object via this name.
- Symbolic Device Name (optional): Exposes the driver interface to the user-mode application.
- Win32 Device Name: The windows device name.

### Page Table Entry

The Page Table Entry (PTE) is a hardware designed structure for the purpose of determining the corresponding memory page privilege access rights. These are the following access rights:

- G: Global
- D: Dirty
- A: Accessed
- C: Cache-Disabled
- W: Write-Through
- U:User/Supervisor
- R: Read/Write
- P: Present

Some malwares such as kernel rootkits take advantage from this structure by using memory-based attacks. 