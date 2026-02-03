IIS tilde directory enumeration is a technique utilised to uncover hidden files.
## ShortName Scanner
```shell
java -jar iis_shortname_scanner.jar 0 5 http://10.129.204.231/
```
Upon executing the tool, it discovers 2 directories and 3 files. However, the target does not permit `GET` access to `http://10.129.204.231/TRANSF~1.ASP`, necessitating the brute-forcing of the remaining filename.
#### Generate Wordlist
```shell
egrep -r '^transf' /usr/share/wordlists/* | sed 's/^[^:]*://' > list.txt
```

#### Gobuster
```shell
gobuster dir -u http://10.129.204.231/ -w /tmp/list.txt -x .aspx,.asp
```