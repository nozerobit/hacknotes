# Analyzing Documents

## Olevba

olevba is a script written in python that allows you to parse OLE and OpenXML as MS Office documents \(word, excel, ...\) to extract VBA code Macros in clear text, find and analyze malicious macros

Installation:

```text
git clone https://github.com/decalage2/oletools
cd oletools
python3 setup.py install
```

Usage:

```text
olevba Currency\ Volume\ Report.xlsm
```

Olevba can find the contents of the document. In some projects, I have found credentials and some macros.

