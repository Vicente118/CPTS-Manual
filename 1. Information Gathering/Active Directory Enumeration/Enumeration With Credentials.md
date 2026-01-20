## From Linux
#### NetExec - Domain User Enumeratiom  
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 --users
```
#### NetExec - Domain Group Enumeration
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```
#### NetExec - Logged On Users
```shell
sudo nxc smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```

---
#### Share Enumeration - Domain Controller
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```
#### Spider_plus
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```
*!* When completed, CME writes the results to a JSON file located at `/tmp/cme_spider_plus/<ip of host>`. We can then dig further to find sensitive files.

---
#### SMBMap To Check Access
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```
#### Recursive List Of All Directories
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```
*!!* `--dir-only` is used to list only directories.

---
#### rpcclient Enumeration
A [Relative Identifier (RID)](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) is a unique identifier (represented in hexadecimal format) utilized by Windows to track and identify objects.
SID of User = SID of Domain + RID of User
#### Enumdomusers
```shell-session
rpcclient $> enumdomusers

user:[administrator] rid:[0x1f4]
user:[guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[lab_adm] rid:[0x3e9]
user:[htb-student] rid:[0x457]
user:[avazquez] rid:[0x458]
user:[pfalcon] rid:[0x459]
...
```
#### RPCClient User Enumeration By RID
```shell
rpcclient $> queryuser 0x457
```

---
#### Windapsearch - Domain Admins
```shell
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```
#### Windapsearch - Privileged Users
```shell
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```

---
## Bloodhound.py
[[Bloodhound]]
