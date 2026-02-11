#### Targets Format
```shell
nxc [protocol] 10.10.10.1
nxc [protocol] 10.10.10.1 10.10.10.2 10.10.10.3
nxc [protocol] 10.10.10.1/24
nxc [protocol] internal.local
nxc [protocol] targets.txt
```
#### List General Options and Protocols
```shell
crackmapexec --help
```
#### View Options Available with any Protocol
```shell
crackmapexec ldap --help
crackmapexec smb --help
```
#### View Available Modules for any Protocol
```shell
crackmapexec ldap -L
```