##### Port 873
#### Probing for Accessible Shares
```
nc -nv 127.0.0.1 873
```
Found: dev
#### Enumerate Share
```
rsync -av --list-only rsync://127.0.0.1/dev
```

#### Get Files in Share
```bash
rsync -av rsync://127.0.0.1/dev
```

If it uses ssh for file transfer:
```bash
rsync -av rsync://127.0.0.1/dev -e "ssh -p22"
```

