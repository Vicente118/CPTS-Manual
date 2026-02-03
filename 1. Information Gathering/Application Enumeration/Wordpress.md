## Discovery 
#### Looking at robots.txt
```shell
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php
Disallow: /wp-content/uploads/wpforms/
```

- WordPress stores its plugins in the `wp-content/plugins` directory.
- Themes are stored in the `wp-content/themes` directory.

## Roles
There are five type of users on a wordpress installation:
1. Administrator: Full Access
2. Editor: Can edit and manage posts.
3. Author: Can publish and edit their own posts.
4. Contributor: Can write post but not publish them.
5. Subscriber: Standard users who can browse posts.

## Enumeration
#### Version
```shell
curl -s http://blog.inlanefreight.local | grep WordPress
```
#### Plugins
```shell
curl -s http://blog.inlanefreight.local/ | grep plugins
```
#### Themes
```shell
curl -s http://blog.inlanefreight.local/ | grep themes
```
#### Users
We can enumerate users with a wordlists on the login page since worpress tells us when a users is valid or not.
## WPScan
We can obtain an API token from [WPVulnDB](https://wpvulndb.com/), which is used by WPScan to scan for PoC and reports.
```shell
sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token dEOFB<SNIP>
```