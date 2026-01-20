#### Wireshark + ARP Filter to Discover Hosts
We can also look at MDNS protocol to see full FQDN of hosts.
#### TCPdump (Useful if we don't have GUI)
```shell
tcpdump -i ens224 
```
#### Fping
```shell
fping -asgq 172.16.5.0/23
```
