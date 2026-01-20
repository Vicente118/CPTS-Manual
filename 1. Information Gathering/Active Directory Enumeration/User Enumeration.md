## Identifying Users (Example without creds)
#### Kerbrute - Internal AD Username Enumeration
```shell
kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```
#### RPCClient
```shell
rpcclient -U'%' 10.10.110.17

rpcclient $> enumdomusers
```
#### enum4linux
```shell
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```
#### NetExec
```shell
nxc smb 172.16.5.5 --users
```
#### LDAP
```shell
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```
#### windapsearch
```shell
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```

*Note*: We can use the tool above with valid credentials to perform the same enumeration, with credentials.