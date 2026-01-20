##### Port 1433
#### Nmap
```bash
nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
```

#### Connecting with Mssqlclient.py (Doesn't work with Exegol)
```bash
python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth
python3 mssqlclient.py Administrator@10.129.201.248 
```
#### dbreaver (GUI that support multiple Databases)
```shell
dbreaver &
```
---
## From Windows
#### SQLCMD
```cmd
sqlcmd -S 10.129.20.13 -U username -P Password123
```
