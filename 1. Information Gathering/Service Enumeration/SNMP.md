##### Port 161 / 162 TCP or UDP

#### Brute Force Community String
```bash
onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt <ip>
```

#### Enumeration
```bash
snmpwalk -v2c -c <community-string> <ip>
```

```bash
braa <community string>@<IP>:.1.3.6.*
```
