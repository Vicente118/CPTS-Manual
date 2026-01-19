##### Port 5985 / 5986
#### Nmap
```
nmap -sV -sC 10.129.201.248 -p5985,5986 --disable-arp-ping -n
```
#### Interact
```bash
evil-winrm -i <ip> -u <user> -p <pass>
```

