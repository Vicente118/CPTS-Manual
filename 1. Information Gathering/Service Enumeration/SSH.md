##### Port 22
#### Interact
##### Password
```bash
ssh <user>@<ip> -p <port>
```
##### Private Key
```bash
chmod 600 id_rsa
ssh <user>@<ip> -i id_rsa -p <port>
```
#### SSH-Audit
```bash
./ssh-audit.py 10.129.14.132
```

#### Change Authentication Method
```bash
ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
```