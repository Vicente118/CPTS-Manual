## Collector 
#### Sharphound on Windows
```powershell
SharpHound.exe --collectionmethods All
```
#### Bloodhound.py
```shell
bloodhound.py --zip -c All -d $DOMAIN -u $USERNAME -p $PASSWORD -dc $DOMAIN_CONTROLLER (-ns NAMESERVER_IP)
```
#### Rusthound
```shell
rusthound --zip -d "$DOMAIN" -i "$DC_IP" -u '$USER@$DOMAIN' -p '$PASSWORD' -o "OUTDIR"
```

## Bloodhoud
#### Run neo4j database
```shell
neo4j start
```
#### Run Bloodhound
```shell
bloodhound &> /dev/null &
```
#### Quick Win
```shell
bhqc.py -u $neo4juser -p $neo4jpassword
```
