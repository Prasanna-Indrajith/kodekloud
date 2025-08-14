# Install and configure Tomcat Server

## Question

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:

a. Install tomcat server on App Server 2.

b. Configure it to run on port 8087.

c. There is a ROOT.war file on Jump host at location /tmp.


Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp02:8087

## Answer


```bash
ssh steve@stapp02

sudo yum -y update

sudo su -

```

install, enable, start Tomcat
```bash
yum install tomcat

systemctl enable tomcat

systemctl start tomcat
```

Configure port:8087 <- Server configuration file is in `/etc/tomcat/server.xml`
```bash
vi /etc/tomcat/server.xml
```

Change Connector's port 8080 into 8087
```server.xml
<Connector port="8087" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Restart and copy the `ROOT.war` file into stapp02 `~/` from **jump server**
```bash
systemctl restart tomcat

exit
exit <- this back into jump server

scp /tmp/ROOT.war steve@stapp02:~/
```

SSH back into stapp02 and copy the file (deploy) into **tomcat** webapps directory (/var/lib/tomcat/webapps/)
```bash
sudo cp ~/ROOT.war /var/lib/tomcat/webapps

sudo systemctl restart tomcat
```

Check deployment is success!
```bash
curl http://stapp02:8087
```


## Additional Details

Configuration files location
```bash
/etc/tomcat/
```

Webapps files location/directory
```bash
/var/lib/tomcat/webapps
```

## Brief Description About the Environment

**What is Tomcat?**

Tomcat is an open-source web server and servlet container developed by the Apache Software Foundation. It is used to deploy and run Java web applications, supporting technologies like Java Servlets, JavaServer Pages (JSP), and WebSockets. Tomcat acts as a lightweight HTTP server and provides the environment for Java code to execute in response to web requests.

