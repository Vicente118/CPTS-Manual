##### Port 139/445 
#### SMBClient
##### Null Session
List shares:
```bash
smbclient -N -L //10.129.14.128
```
Connect to share:
```bash
smbclient -N //10.129.14.128/<share>
```
##### User Session
List shares:
```bash
smbclient -L //10.129.14.128
```
Connect to share:
```bash
smbclient //10.129.14.128/<share>
```
##### Download from SMB
```smb
smb: \> get <file>
```

---
#### Brute Forcing Users RIDs
```bash
samrdump.py <ip>
```

---
#### SMBMap

##### Null Session
```bash
smbmap -H 10.129.14.128
```
##### User Session
```bash
smbmap -H 172.16.7.3 -u <user> -p <password>
```

---
#### Enum4Linux-ng
```bash
enum4linux-ng -A "$TARGET"
```
(Can specify User and Password)