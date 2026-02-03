## Discovery
```shell
curl -s http://dev.inlanefreight.local/ | grep Joomla
```

Joomla robots.txt often looks like this:
```shell
# If the Joomla site is installed within a folder
# eg www.example.com/joomla/ then the robots.txt file
# MUST be moved to the site root
# eg www.example.com/robots.txt
# AND the joomla folder name MUST be prefixed to all of the
# paths.
# eg the Disallow rule for the /administrator/ folder MUST
# be changed to read
# Disallow: /joomla/administrator/
#
# For more information about the robots.txt standard, see:
# https://www.robotstxt.org/orig.html

User-agent: *
Disallow: /administrator/
Disallow: /bin/
Disallow: /cache/
Disallow: /cli/
Disallow: /components/
Disallow: /includes/
Disallow: /installation/
Disallow: /language/
Disallow: /layouts/
Disallow: /libraries/
Disallow: /logs/
Disallow: /modules/
Disallow: /plugins/
Disallow: /tmp/
```
#### Fingerprint Version
```shell
curl -s http://dev.inlanefreight.local/README.txt | head -n 5
```

In certain Joomla installs, we may be able to fingerprint the version from JavaScript files in the `media/system/js/` directory or by browsing to `administrator/manifests/files/joomla.xml`
```shell
curl -s http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml | xmllint --format -
```
The `cache.xml` file can help to give us the approximate version. It is located at `plugins/system/cache/cache.xml`

## Enumeration
Plugin-based scanner:
```shell
droopescan scan joomla --url http://dev.inlanefreight.local/
```

Joomla Scanner:
```shell
python2.7 joomlascan.py -u http://dev.inlanefreight.local
```

Brute force login (For very weak password):
```shell
python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```