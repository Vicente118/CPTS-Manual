## Discovery
#### Version
- We can request an invalind page to reveal version.
- We can also access `/docs`
```shell
curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat 
```
#### Default folder structure
```shell
├── bin
├── conf
│   ├── catalina.policy
│   ├── catalina.properties
│   ├── context.xml
│   ├── tomcat-users.xml
│   ├── tomcat-users.xsd
│   └── web.xml
├── lib
├── logs
├── temp
├── webapps
│   ├── manager
│   │   ├── images
│   │   ├── META-INF
│   │   └── WEB-INF
|   |       └── web.xml
│   └── ROOT
│       └── WEB-INF
└── work
    └── Catalina
        └── localhost
```
- The `bin` folder stores scripts and binaries needed to start and run a Tomcat server.
- The `conf` folder stores various configuration files used by Tomcat.
- The `tomcat-users.xml` file stores user credentials and their assigned roles.
- The `lib` folder holds the various JAR files needed for the correct functioning of Tomcat.
- The `logs` and `temp` folders store temporary log files.
- The `webapps` folder is the default webroot of Tomcat and hosts all the applications.
- The `work` folder acts as a cache and is used to store data during runtime.

Each folder inside `webapps` is expected to have the following structure.
```shell
webapps/customapp
├── images
├── index.jsp
├── META-INF
│   └── context.xml
├── status.xsd
└── WEB-INF
    ├── jsp
    |   └── admin.jsp
    └── web.xml
    └── lib
    |    └── jdbc_drivers.jar
    └── classes
        └── AdminServlet.class   
```
The most important file among these is `WEB-INF/web.xml`, which is known as the deployment descriptor.
#### Web.xml
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>

<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">

<web-app>
  <servlet>
    <servlet-name>AdminServlet</servlet-name>
    <servlet-class>com.inlanefreight.api.AdminServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>AdminServlet</servlet-name>
    <url-pattern>/admin</url-pattern>
  </servlet-mapping>
</web-app>
```
The AdminServlet is mapped to the class `com.inlanefreight.api.AdminServlet` and is defined in `classes/com/inlanefreight/api/AdminServlet.class`. Just change dot to `/` and add classes prefix and `.class` suffix.
#### tomcat-users.xml
Used to allow or disallow access to the `/manager` and `host-manager` admin pages.
The file shows us what each of the roles `manager-gui`, `manager-script`, `manager-jmx`, and `manager-status` provide access to. In this example, we can see that a user `tomcat` with the password `tomcat` has the `manager-gui` role, and a second weak password `admin` is set for the user account `admin`

## Enumeration
Fuzzing:
```shell
gobuster dir -u http://web01.inlanefreight.local:8180/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```