## From Linux
#### Netexec
```shell
nxc smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```
#### RPC CLient
```shell
> rpcclient -U "" -N 172.16.5.5

rpcclient $> querydominfo
Domain:		INLANEFREIGHT
Server:		
Comment:	
Total Users:	3650
Total Groups:	0
Total Aliases:	37
Sequence No:	1
Force Logoff:	-1
Domain Server State:	0x1
Server Role:	ROLE_DOMAIN_PDC
Unknown 3:	0x1


rpcclient $> getdompwinfo
min_password_length: 8
password_properties: 0x00000001
DOMAIN_PASSWORD_COMPLEX
```
#### enum4linux-ng
```shell
enum4linux-ng -P 172.16.5.5 -oA ilfreight
```
#### LDAP
```shell
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```
**Note**: In newer versions of `ldapsearch`, the `-h` parameter was deprecated in favor for `-H`.

---
## NULL Session from Windows
#### Establish a null session from windows
```cmd
C:\htb> net use \\DC01\ipc$ "" /u:""
The command completed successfully.
```
#### Error: Account is Disabled
```cmd
C:\htb> net use \\DC01\ipc$ "" /u:guest
System error 1331 has occurred.

This user can't sign in because this account is currently disabled.
```
#### Error: Account is locked out (Password Policy)
```cmd
C:\htb> net use \\DC01\ipc$ "password" /u:guest
System error 1909 has occurred.

The referenced account is currently locked out and may not be logged on to.
```

## Password Policy from Windows
#### net.exe
```cmd
net accounts
```
#### PowerView
```powershell
PS C:\htb> import-module .\PowerView.ps1
PS C:\htb> Get-DomainPolicy
```