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

---
#### Mount share on Linux
```shell
sudo mkdir /mnt/Finance

sudo mount -t cifs -o username=plaintext,password=Password123,domain=. //192.168.220.129/Finance /mnt/Finance
```
#### Find Pattern in Filename
```shell
find /mnt/Finance/ -name *cred*
```
#### Find Pattern in File
```shell
grep -rn /mnt/Finance/ -ie cred
```

---
## From Windows

```smd
dir \\192.168.220.129\Finance\
```
#### Net Use
```cmd
net use n: \\192.168.220.129\Finance /user:plaintext Password123
```
Map the share to n: drive
#### Number of files 
```cmd
dir n: /a-d /s /b | find /c ":\"
```
#### Search for pattern in filename
```cmd
dir n:\*cred* /s /b

dir n:\*secret* /s /b
```
#### Search for pattern in file
```cmd
findstr /s /i cred n:\*.*
```

#### Windows PowerShell
```powershell
Get-ChildItem \\192.168.220.129\Finance\
```

Instead of `net use`, we can use `New-PSDrive` in PowerShell
```powershell
New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem"
```
To provide a username and password with Powershell, we need to create a [PSCredential object](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.pscredential)
```powershell
$username = 'plaintext'
$password = 'Password123'
$secpassword = ConvertTo-SecureString $password -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred
```
#### Windows PowerShell - GCI
```powershell
PS C:\htb> N:
PS N:\> (Get-ChildItem -File -Recurse | Measure-Object).Count
```
Count files in N:
#### Pattern in filename
```powershell
Get-ChildItem -Recurse -Path N:\ -Include *cred* -File
```
#### Pattern in file
```powershell
Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List
```
