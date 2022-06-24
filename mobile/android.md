# Android

## Basics

This section covers the fundamentals and basics of the Android OS.

### File System

- `/boot`: It contains the Android kernel and `ramdisk`. Basically, everything that's required for an Android device to boot up after being turned on. Only when it is absolutely required should you proceed to wipe this partition from recovery. If you choose to do this, make sure to reboot the phone only after adding a new boot partition and after the partition has been erased. A ROM's /boot partition may be the new partition.
- `/system`: The whole Android OS is here. This applies to both the system apps that come pre-installed on Android devices and the Android GUI. If this partition is wiped then only recovery and bootloader modes will be accessible on the device.
- `/recovery`: Is made for backups. It enables the device to boot into recovery mode where data backup, data erasing, phone restoration to factory settings, and other maintenance tasks can be carried out.
- `/data/data`: Where all the apps are installed by default.
- `/cache`: The frequently used app data and components are kept in this partition. When you wipe this partition, the device will just delete the cache that has accumulated; as soon as you restart it and use it again, the cache will be rebuilt. Additionally, clearing the cache can sometimes resolve problems and free up some space on your smartphone.
- `/misc`: All of the various system settings, typically on/off switches, are located in this partition. The parameters could include hardware configuration, USB configuration, carrier or region ID, etc. This partition is crucial because if it is corrupted or lost, it could make a number of gadget functionalities malfunction. The Android device may also fail to boot at all if the `/misc` partition is absent.
- `/sdcard`: Users have access to this partition as the storage location for their data. You might even have multiple SD card partitions, depending on your device. The `/sdcard` partition refers to the internal storage device if your device has an external SD card port. The external SD card will be given a different partition, either a new partition called `/sdcard2` or a distinct directory within the `/sdcard` partition itself, such as `/sdcard/sd`. As long as you have backed up any crucial data or documents that might be present on the partition with the external SID card storage, wiping it is totally secure.
- `/sd-ext`: Only some devices, usually those with custom ROMs or other modifications have this additional partition of the sdcard partition. It may typically be utilized with specific ROMs that support unique features like APP2SD+ or data2ext. A portion of the user data and settings that are generally kept on the `/data` partition are stored here, which is effectively the `/data` partition of the `/sdcard`. For gadgets with less internal memory, this is helpful.

### Change User

We can change the user:

```sh
su username
```

### Android Studio

Android Studio provides the fastest tools for building apps on every type of Android device.

{% embed url="https://developer.android.com/studio" %}

The documentation can found here:

{% embed url="https://developer.android.com/studio/intro" %}

The installation instructions for each supported operating system can be found here:

{% embed url="https://developer.android.com/studio/install" %}

## Reverse Engineering

This section covers the fundamentals and basics of reverse engineering Android applications.

### Decompile

We can decompile an apk file with `apktool`:

```sh
apktool d filename.apk
```

We can use JD-GUI to decompile a `.jar` file:

- Open -> filename.jar

{% embed url="http://java-decompiler.github.io/" %}

### Convert DEX to Java Classes (JAR)

We can convert a `.dex` file into a java class file (`.jar`):

```sh
de2j-dex2jar.sh classes.dex 
```

Here is the repo:

{% embed url="https://github.com/pxb1988/dex2jar" %}

### Disassembler

#### Smali

smali/baksmali is an assembler/disassembler for the dex format used by dalvik, Android's Java VM implementation. The syntax is loosely based on Jasmin's/dedexer's syntax, and supports the full functionality of the dex format (annotations, debug info, line info, etc.)

{% embed url="https://github.com/JesusFreke/smali" %}

Dalvik EXecutable (dex) files can be decompiled to obtain a low-level code called Dalvik Bytecode. Nowadays, `apktool` already incorporates smali/baksmali, so it will be enough to unpack the `.apk` file with `apktool` to get the Smali code.

Smali has a wiki:

{% embed url="https://github.com/JesusFreke/smali/wiki" %}

We can use smali to disassemble a dex (Dalvik EXecutable) file:

```sh
java -jar baksmali-version.jar classes.dex
```

### Static Analysis



### Dynamic Runtime Analysis

Dynamic Analysis is a technique for analyzing software's behavior while it runs. This encompasses not only analysis but also the equipment itself. Some of these actions do not require a detailed examination for security assessment.

We can perform dynamic analysis by using debuggers such as the following:

- **Android Debug Monitor (ADT)**
- **Dalvik Debug Monitor Server (DDMS)**: The Dalvik Debug Monitor Server (DDMS) is a debugging tool that provides port-forwarding services, device screen capture, thread and heap information, logcat, process, and radio state information, incoming call and SMS spoofing, location data spoofing, and more. 

## ADB - Android Debug Bridge

Android Debug Bridge \(adb\) is a versatile command-line tool that lets you communicate with a device. The adb command facilitates a variety of device actions, such as installing and debugging apps, and it provides access to a Unix shell that you can use to run a variety of commands on a device. It is a client-server program that includes three components:

* **A client**, which sends commands. The client runs on your development machine. You can invoke a client from a command-line terminal by issuing an adb command.
* **A daemon \(adbd\)**, which runs commands on a device. The daemon runs as a background process on each device.
* **A server**, which manages communication between the client and the daemon. The server runs as a background process on your development machine.

`adb` is included in the Android SDK Platform-Tools package. You can download this package with the [SDK Manager](https://developer.android.com/studio/intro/update#sdk-manager), which installs it at `android_sdk/platform-tools/`. Or if you want the standalone Android SDK Platform-Tools package, you can [download it here](https://developer.android.com/studio/releases/platform-tools)

{% embed url="https://developer.android.com/studio/command-line/adb" %}


We can execute a shell:

```sh
adb shell
```

We can connect remotely:

```text
❯ ss -tnlp
State                   Recv-Q                  Send-Q                                   Local Address:Port                                     Peer Address:Port                  Process
LISTEN                  0                       128                                          127.0.0.1:5555                                          0.0.0.0:*                      users:(("ssh",pid=1126086,fd=5))
LISTEN                  0                       128                                              [::1]:5555                                             [::]:*                      users:(("ssh",pid=1126086,fd=4))

# Connect to the port
❯ adb connect 127.0.0.1:5555
* daemon not running; starting now at tcp:5037
* daemon started successfully
connected to 127.0.0.1:5555

# Error
❯ adb shell
error: more than one device/emulator

# Error Fix
❯ adb devices
List of devices attached
127.0.0.1:5555  device
emulator-5554   device

# Connect to a device
❯ adb -s emulator-5554 shell
x86_64:/ $ whoami
shell
x86_64:/ $ su
:/ # id
uid=0(root) gid=0(root) groups=0(root) context=u:r:su:s0
```

## Display and Control

The srcpy application provides display and control of Android devices connected on USB \(or [over TCP/IP](https://www.genymotion.com/blog/open-source-project-scrcpy-now-works-wirelessly/)\). It does not require any _root_ access. It works on _GNU/Linux_, _Windows_ and _macOS_.

{% embed url="https://github.com/Genymobile/scrcpy" %}

## Proxies

We can set up BurpProxy for pentesting purposes. Portswigger already has documentation written here:

{% embed url="https://portswigger.net/support/configuring-an-android-device-to-work-with-burp" %}

## Tapjacking

