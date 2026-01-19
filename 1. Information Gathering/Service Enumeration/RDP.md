##### Port 3389
#### Nmap 
```bash
nmap -sV -sC 10.129.201.248 -p3389 --script rdp*
```
#### Interact
```bash
xfreerdp /u:<user> /p:<pass> /v:<ip> /drive:share,/path/to/share /size:1920x1080 +clipboard (/cert-ignore)

xfreerdp /u:<user> /pth:<NT_HASH> /v:<ip> /drive:share,/path/to/share /size:1920x1080 +clipboard (/cert-ignore)
```
