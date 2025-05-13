# Maven Web Application Deployment on Tomcat

This project demonstrates how to build a simple Java web application using Maven and deploy it to an Apache Tomcat server manually.

---

## - Prerequisites

Ensure the following packages are installed on your system:

```bash
yum install git -y
yum install maven -y
```

---

## - Setup Instructions

### 1. Clone the Application Repository

```bash
git clone https://github.com/gou8m/maven-tomcat.git
cd maven-tomcat
```

### 2. Download and Extract Apache Tomcat

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.105/bin/apache-tomcat-9.0.105.tar.gz
tar -xzvf apache-tomcat-9.0.105.tar.gz
mv apache-tomcat-9.0.105 tomcat
```

---

## - Build the Application

Navigate to the project directory and run:

```bash
mvn clean package
```

The output WAR file will be located in:

```bash
webapp/target/webapp.war
```

---

## - Deploy to Tomcat

Copy the WAR file to Tomcat’s `webapps` directory:

```bash
cp webapp/target/webapp.war /root/tomcat/webapps
```

Start Tomcat:

```bash
cd /root/tomcat/bin
sh shutdown.sh
sh startup.sh
```

Access the application in your browser:

```
http://<server-ip>:8080/webapp
```

---

## - Enable Tomcat Manager Access (For Local Testing)

Tomcat restricts access to the **Manager** and **Host Manager** apps. Here’s how to enable them:

### 1. Allow External Access

Comment out the `RemoteAddrValve` lines in:

**Host Manager**
```bash
vi /root/tomcat/webapps/host-manager/META-INF/context.xml
```

**Manager**
```bash
vi /root/tomcat/webapps/manager/META-INF/context.xml
```

Example:
```xml
<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
```

### 2. Configure Users & Roles

Edit the users file:

```bash
vi /root/tomcat/conf/tomcat-users.xml
```

Add the following within `<tomcat-users>`:

```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>

<user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status"/>
<user username="deployer" password="deployer" roles="manager-script"/>
<user username="tomcat" password="s3cret" roles="manager-gui"/>
```

---

## - Directory Structure

```
maven-tomcat/
├── webapp/
│   ├── src/
│   ├── target/
│   └── pom.xml
└── tomcat/
    ├── bin/
    ├── conf/
    ├── webapps/
    └── logs/
```

---

## - Notes

- This setup is for **manual testing** only.
- For **production**, use secure passwords, limit access to the manager apps, and deploy via CI/CD.
- HTML or JSP pages go inside `src/main/webapp/`.

---

## - Useful Commands

Start Tomcat:
```bash
sh /root/tomcat/bin/startup.sh
```

Stop Tomcat:
```bash
sh /root/tomcat/bin/shutdown.sh
```

Check logs:
```bash
tail -f /root/tomcat/logs/catalina.out
```

---

