## 1 - Information Gathering

> [!summary]- 1.1 Tools
> - [[Nmap]]
> - [[tcpdump]]

> [!summary]- 1.2 Services Enumeration  
> - [[FTP]]
> - [[SMB]]
> - [[NFS]]
> - [[DNS]]
> - [[SMTP]]
> - [[IMAP - POP3]]
> - [[SNMP]]
> - [[MySQL]]
> - [[MSSQL]]
> - [[Oracle TNS]]
> - [[IPMI]]
> - [[SSH]]
> - [[RSync]]
> - [[R-Services]]
> - [[RDP]]
> - [[WinRM]]
> - [[WMI]]
> - [[RPC]]

> [!summary]- 1.3 Web Enumeration
> - [[Virtual Hosts]]
> - [[Fingerprinting]]
> - [[Robots.txt]]
> - [[ReconSpider.py]]
> - [[FinalRecon.py]]

> [!summary]- 1.4 Application Enumeration
> [[Wordpress]]
> [[Joomla]]
> [[Drupal]]
> [[Tomcat]]
> [[Jenkins]]
> [[Splunk]]
> [[PRTG Network Monitor]]
> [[osTicket]]
> [[GitLab]]
> [[ColdFusion]]
> [[IIS Tilde Enumeration]]

> [!summary]- 1.5 Active Directory Enumeration
> [[Hosts Enumeration]]
> [[User Enumeration]]
> [[Enumeration With Credentials]]
> [[SMB Enumeration]]
> [[Password Policy Enumeration]]
> [[Enumerating Security Controls]]
> [[ACL Enumeration]]
> [[Bloodhound]]
> [[Trust Relationships Enumeration]]
> [[Powerview]]

---
## 2 - Pre-Exploitation

> [!summary]- 2.1 Shells
> [[Bind Shells]]
> [[Reverse Shells]]
> [[Web Shells]]
> [[Escape Restricted Shells]]

> [!summary]- 2.2 Password Wordlist Generation
> [[Password Wordlist Generation]]

> [!summary]- 2.3 Tools
>[[MSFConsole]]
>[[MSFVenom]]
>[[Searchsploit]]

---
## 3 - Exploitation

> [!summary]- 3.1 Service Exploitation
> - [[FTP 21]]
> - [[SMB 139,445]]
> - [[DNS 53]]
> - [[SMTP 25,465,587]]
> - [[IMAP - POP3 143,993 - 110,995]]
> - [[MySQL 3306]]
> - [[MSSQL 1433,2433]]
> - [[RDP 3389]]

> [!summary]- 3.2 Web Exploitation
> - [[HTTP Basic Auth Brute Force]]
> - [[Login Form Brute Force]]
> - [[XSS]]
> - [[SQL Injection]]
> - [[LFI]]
> - [[Command Injection]]
> - [[HTTP Verb Tampering]]
> - [[IDOR]]
> - [[XXE]]
> - [[CGI Shellshock]]
> - [[Mass Assignement]]
> - [[Tools]]

> [!summary]- 3.3 Application Exploitation
> - [[Wordpress Exploitation]]
> - [[Joomla Exploitation]]
> - [[Drupal Exploitation]]
> - [[Tomcat Exploitation]]
> - [[Jenkins Exploitation]]
> - [[Splunk Exploitation]]
> - [[PRTG Network Monitor Exploitation]]
> - [[osTicket Exploitation]]
> - [[GitLab Exploitation]]
> - [[ColdFusion Exploitation]]
> - [[Nagios XI Exploitation]]
> - [[DotNetNuke Exploitation]]
> - [[Other Applications]]

> [!summary]- 3.4 Active Directory Attacks
> - [[LLMNR & NBT-NS Poisoning]]
> - [[Password Spraying & Credential Stuffing]]
> - [[Kerberoasting from Linux]]
> - [[Kerberoasting from Windows]]
> - [[ASREPRoasting]]
> - [[Group Policy Preferences (GPP) Passwords]]
> - [[Group Policy Object (GPO) Abuse]]
> - [[ACL Abuse]]
> - [[DCSync]]
> - [[noPac]]
> - [[PrintNightmare]]
> - [[PetitPotam]]
> - [[Misconfigurations]]
> - [[Trust Abuse Attacking Child -> Parent Trust]]
> - [[Trust Abuse Attacking Cross Forest Trust]]

---
## 4 - Post-Exploitation

> [!summary]- 4.1 File Transfers
> [[Linux -> Windows]]
> [[Linux -> Linux]]
> [[Windows -> Linux]]

> [!summary]- 4.2 Fully Interactive TTY Shell
> [[TTY Shell]]

> [!summary]- 4.3 Linux Privilege Escalation
> [[Host Recon]]
> [[Privileged Groups]]
> [[Sudo Abuse]]
> [[SUID - GUID]]
> [[Cronjob Abuse]]
> [[Capabilities]]
> [[Wildcard Injection]]
> [[Python Library Hijacking]]
> [[Credential Hunring]]
> [[PATH Abuse]]
> [[Docker Group]]
> [[Kubernetes]]
> [[Passwd - Shadow]]
> [[Escape Restricted Shells]]
> [[Tmux Session Hijacking]]
> [[Shared Object Hijacking]]
> [[PwnKit]]
> [[LogRotten]]
> [[Kubernetes]]
> [[Payloads]]

> [!summary]- 4.4 Windows Privilege Escalation
> [[Host Recon]]
> [[Rights & Privileges]]
> [[Users & Groups]]
> [[Legacy Operating Systems]]
> [[SeImpersonate & SeAssignPrimaryToken]]
> [[SeDebugPrivilege]]
> [[SeTakeOwnerShipPrivilege]]
> [[Group -> Backup Operators]]
> [[Group -> Event Log Readers]]
> [[Group -> DnsAdmins]]
> [[Group -> Hyper-V Administrators]]
> [[Group -> Print Operators]]
> [[Group -> Server Operators]]
> [[User & Computer Description Field]]
> [[Scheduled Tasks]]
> [[Credential Hunting]]
> [[Always Install Elevated]]
> [[Unquoted Service Path]]
> [[Vulnerable Services]]
> [[Cookie Stealing]]
> [[Weak Service Permissions]]
> [[Permissive Registry ACLs]]
> [[Modifiable Registry Autorun Binary]]
> [[Kernel Exploits]]
> [[VMDK, VHD & VHDX Files]]
> [[Malicious SCF and lnk Files]]
> [[Restic Backup Attack]]
> [[Citrix Breakout]]
> [[Bypassing UAC]]
> [[Payloads]]

> [!summary]- 4.5 Tools & Misc
> [[Hashcat]]
> [[John The Ripper]]
> [[Netcat]]
> [[Mouting Bitlocker Drive (.vhd)]]

---
## 5 - Lateral Movement & Pivoting

> [!summary]- 5.1 Linux Lateral Movement
> [[Linux Authentication]]
> [[Credentials Hunting in Linux]]
> [[Pass-The-Ticket from Linux]]

> [!summary]- 5.2 Windows Lateral Movement
> [[SAM, SYSTEM and SECURITY Dump]]
> [[LSASS Dump]]
> [[Attacking Credential Manager]]
> [[Credential Hunting in Windows]]
> [[Credential Hunting in Network Shares]]
> [[Pass The Hash]]

> [!summary]- 5.3 Active Directory Lateral Movement
> [[NTDS.dit Dump]]
> [[RDP Lateral Movement]]
> [[WinRM - Powershell Remoting]]
> [[MSSQL Server]]
> [[Double Hop Problem]]
> [[Password Spraying & Credential Stuffing]]
>  [[Pass-The-Ticket from Windows]]
>  [[Pass-The-Certificate]]

> [!summary]- 5.4 Pivoting
> [[Ligolo]]

> [!summary]- 5.5 Misc
> [[Credentials Hunting in Network Traffic]]

---
## 6 - NetExec 


---
## 7 - BloodyAD