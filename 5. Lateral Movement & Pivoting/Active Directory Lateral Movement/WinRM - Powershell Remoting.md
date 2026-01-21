We can again use the PowerView function `Get-NetLocalGroupMember` to the `Remote Management Users` group.
#### Enumerating the Remote Management Users Group
```powershell
PS C:\htb> Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"

ComputerName : ACADEMY-EA-MS01
GroupName    : Remote Management Users
MemberName   : INLANEFREIGHT\forend
SID          : S-1-5-21-3842939050-3880317879-2865463114-5614
IsGroup      : False
IsDomain     : UNKNOWN
```

We can also utilize this custom `Cypher query` in BloodHound to hunt for users with this type of access.
```cypher
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2
```
#### Establishing WinRM Session from Windows
```powershell
PS C:\htb> $password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force

PS C:\htb> $cred = new-object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password)

PS C:\htb> Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred

[ACADEMY-EA-MS01]: PS C:\Users\forend\Documents> hostname
ACADEMY-EA-MS01
```
#### Connecting to a Target with Evil-WinRM and Valid Credentials
```shell
evil-winrm -i 10.129.201.234 -u forend
```


