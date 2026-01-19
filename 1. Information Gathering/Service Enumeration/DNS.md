#### SOA Query
##### Information about DNS zone and email address of administrative contact
```bash
dig soa www.inlanefreight.com
```
`awsdns-hostmaster.amazon.com ` -> `awsdns-hostmaster@amazon.com`

---
#### NS Query
##### DNS nameserver
```bash
dig ns inlanefreight.htb @10.129.14.128
```

---
#### Version Query
```bash
dig CH TXT version.bind 10.129.120.85
```

---
#### ANY Query
##### Shows all available records
```bash
dig any inlanefreight.htb @10.129.14.128
```

---
#### DNS Zone Transfer
```bash
dig axfr inlanefreight.htb @10.129.14.128
```
