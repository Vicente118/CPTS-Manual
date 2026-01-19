#### Common credential patterns
- Keywords: `passw`, `user`, `token`, `key`, and `secret`
- File extensions: `.ini`, `.cfg`, `.env`, `.xlsx`, `.ps1`, and `.bat`
- Filename containing: `config`, `user`, `passw`, `cred`, or `initial`
- Filename containing Domain name
- Keyword depending of company language

---
## Hunting from Windows
#### Snaffler
```powershell
Snaffler.exe -s
```
**Interesting Parameters**: 
- `-U` : Retrieve a list of users from AD and searches for references to them in files
- `-i` & `-n`: Allow to specify which shares should be included in the search

---
#### PowerHuntShares
Provides HTML report of the scan.
```powershell
PS C:\Users\Public\PowerHuntShares> Invoke-HuntSMBShares -Threads 100 -OutputDirectory c:\Users\Public
```

---
## Hunting from Linux
#### MANSPIDER
```shell
docker run --rm -v ./manspider:/root/.manspider blacklanternsecurity/manspider 10.129.234.121 -c 'passw' -u 'mendres' -p 'Inlanefreight2025!'
```

---
#### NetExec
```shell
nxc smb 10.129.234.121 -u jbader -p 'ILovePower333###' --spider Marketing --content --pattern "passw"
```
