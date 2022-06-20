# Android

## Android



### Dynamic Runtime Analysis

Dynamic Analysis is a technique for analyzing software's behavior while it runs. This encompasses not only analysis but also the equipment itself. Some of these actions do not require a detailed examination for security assessment.

We can perform dynamic analysis by using debuggers such as the following:

- **Android Debug Monitor (ADT)**
- **Dalvik Debug Monitor Server (DDMS)**: The Dalvik Debug Monitor Server (DDMS) is a debugging tool that provides port-forwarding services, device screen capture, thread and heap information, logcat, process, and radio state information, incoming call and SMS spoofing, location data spoofing, and more. 

### ADB - Android Debug Bridge

Android Debug Bridge \(adb\) is a versatile command-line tool that lets you communicate with a device. The adb command facilitates a variety of device actions, such as installing and debugging apps, and it provides access to a Unix shell that you can use to run a variety of commands on a device. It is a client-server program that includes three components:

* **A client**, which sends commands. The client runs on your development machine. You can invoke a client from a command-line terminal by issuing an adb command.
* **A daemon \(adbd\)**, which runs commands on a device. The daemon runs as a background process on each device.
* **A server**, which manages communication between the client and the daemon. The server runs as a background process on your development machine.

`adb` is included in the Android SDK Platform-Tools package. You can download this package with the [SDK Manager](https://developer.android.com/studio/intro/update#sdk-manager), which installs it at `android_sdk/platform-tools/`. Or if you want the standalone Android SDK Platform-Tools package, you can [download it here](https://developer.android.com/studio/releases/platform-tools)

{% embed url="https://developer.android.com/studio/command-line/adb" %}

#### ADB Example

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

### scrcpy

 This application provides display and control of Android devices connected on USB \(or [over TCP/IP](https://www.genymotion.com/blog/open-source-project-scrcpy-now-works-wirelessly/)\). It does not require any _root_ access. It works on _GNU/Linux_, _Windows_ and _macOS_.

{% embed url="https://github.com/Genymobile/scrcpy" %}





