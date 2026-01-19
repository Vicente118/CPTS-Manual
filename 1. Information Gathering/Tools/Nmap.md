#### Scan Network Range
```bash
nmap 10.10.10.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```
#### Default Scan
```bash
nmap 10.10.10.10 
```

#### Options
`-p` : Specify port
`-sC` : Default scripts scanning
`-sV` : Version scanning
`-Pn` : Disable ICMP packets
`-sU` : UDP scan
`-sT` : TCP scan
`--script` : Specify script to run nmap with
`-sA` : ACK scan
`-sS` : SYN scan
