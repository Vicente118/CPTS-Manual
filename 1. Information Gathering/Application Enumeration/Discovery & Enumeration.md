## Using EyeWitness
EyeWitness can take the XML output from both Nmap and Nessus and create a report with screenshots of each web application present on the various ports using Selenium.
```shell
eyewitness --web -x web_discovery.xml -d inlanefreight_eyewitness
```

---
## Using Aquatone
[Aquatone](https://github.com/michenriksen/aquatone), as mentioned before, is similar to EyeWitness and can take screenshots when provided a `.txt` file of hosts or an Nmap `.xml` file with the `-nmap` flag.
```shell
cat web_discovery.xml | ./aquatone -nmap
```
