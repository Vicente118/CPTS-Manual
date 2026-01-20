##### Port 3306
#### Nmap
```bash
nmap 10.129.14.128 -sV -sC -p3306 --script mysql*
```
#### Interact
```bash
mysql -u <username> -p<password> -h <ip>
```

---
#### Interact Form Windows
```cmd
mysql.exe -u username -pPassword123 -h 10.129.20.13
```
