# Bypassing EDR

There a-lot of techniques that are used to bypass EDRs and AVs. However, a few of them are very common and are often used in red-team engagements. We'll go over some basic EDR bypass methods.

## Hooking 

Hooking a program can be used to determine if the code is malicious or not.

### Userland API Hooking

All API calls including their arguments made by your application (such as CreateFile,ReadFile, OpenProcess, and so on) are intercepted and analyzed by AVs/EDRs, which is then used to determine if the program's action/intent is malicious or not.

A few EDR vendors hijack/modify function definitions (APIs) found in Windows DLLs such as kernel32/kernelbase and ntdll to hook userland APIs.

## Unhooking

Unhooking is used to restore the program original state.

## DLLs Hooks

We could attempt to bypass EDRs/AVs by reading the `.text section` of `ntdll.dll` from disk and putting it on **top** of the `.text section` of the `ntdll.dll` that is mapped in memory, any DLL loaded in memory can be unhooked.

## Packing

Packing a program changes the Original Entry Point (OEP) of the program. It also compresses files and thereofore reduces their size. It also makes it harder for reverse engineers, malware analysts, and EDRs/AVs vendors to detect the program code. There are a-lot of packers in the wild but as an example we can use the well-known UPX packer.

We could pack a binary with the following:

```text
.\upx.exe -9 -o .\nc-packed.exe .\nc.exe
```

## Syscalls

AV/EDR solutions typically use userland Windows APIs to determine whether or not the code being executed is malicious. However, by writing our own functions that invoke syscalls directly, we can bypass hooked functions.

We can define syscalls by using the MASM assembler and declaring C functions.

## Impersonation / Masquerading

The Process Environment Block (PEB) contains the information of the process such as arguments, image base address, loaded modules, and others. However, it is readable and writable from the userland. Since it is writable we could attempt to modify some values in the PEB and impersonate another program.

## Timestomping

Timestomping is used to forge the file creation date, such as the following:

```text
.\timestomp.exe .\nc.exe -c "Monday 10/25/2010 10:19:00 AM"
```

## Encoding

Encoding the code could bypass some AVs/EDRs but is not as effective as encryption.

## Encryption

Encrypting the code makes it really hard to for some AVs/EDRs to analyze the code.
