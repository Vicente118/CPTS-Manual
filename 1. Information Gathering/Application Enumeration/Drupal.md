## Discovery 
A Drupal website can be identified in several ways, including by the header or footer message `Powered by Drupal`, the standard Drupal logo, the presence of a `CHANGELOG.txt` file or `README.txt file`, via the page source, or clues in the robots.txt file such as references to `/node`.
```shell
curl -s http://drupal.inlanefreight.local | grep Drupal
```

Drupal indexes its content using nodes. A node can hold anything such as a blog 
post, poll, article, etc. The page URIs are usually of the form `/node/<nodeid>`.

*Note*: Not every Drupal installation will look the same or display the login page or even allow users to access the login page from the internet.
## Roles
1. Administrator: Full access
2. Authenticated User: Adding articles based on permissions
3. Anonymous: Visitors that read posts
## Enumeration
Check for `README.txt` or `CHANGELOG.txt`
#### Droopescan
```shell
droopescan scan drupal -u http://drupal.inlanefreight.local
```
