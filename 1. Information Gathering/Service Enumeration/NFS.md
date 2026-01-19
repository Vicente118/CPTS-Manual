##### Port 2049

#### Nmap Script
```bash
nmap --script /usr/share/nmap/scripts/nfs* 10.129.14.128 -sV -p111,2049
```
Retrieves a list of all currently running RPC services, their names and descriptions, and the ports they use.

---
#### Show Available NFS Shares
```bash
showmount -e 10.129.14.128
```
#### Mounting NFS Share
Mounting:
```bash
mkdir target-NFS
sudo mount -t nfs 10.129.14.128:/path/to/share ./target-NFS/ -o nolock
cd target-NFS
```
Listing Usernames & Group Names:
```bash
ls -l /mnt/nfs/
```
List UIDs & GUIDs
```bash
ls -n /mnt/nfs/
```

#### Unmounting
```bash
sudo umount ./target-NFS
```

