### Practice 1
- Create Instance
- Allocate Elastic IP address
	- Associate Elastic IP address
- Connect to your EC2 instance
	- ssh -i "your_key_file.pem" ec2-user@your_public_dns
- Installing JAVA
	- sudo yum update
	- java -version
	- yum list java*
	- sudo yum install java-1.8.0
	- java -version
	- choose java version in case of multiple JDKs
		- sudo /usr/sbin/alternatives --config java
		- sudo /usr/sbin/alternatives --config javac
- Installing TOMCAT
	- sudo yum update -y
	- sudo su
	- wget https://apache.inspire.net.nz/tomcat/tomcat-9/v9.0.41/bin/apache-tomcat-9.0.41.tar.gz
	- tar -zvxf apache-tomcat-9.0.41.tar.gz
	- cd apache-tomcat-9.0.41
	- cd bin
	- ./startup.sh
	- ./shutdown.sh
	- cd /home/ec2-user
	- sudo ln -s /home/ec2-user/apache-tomcat-9.0.41/bin/startup.sh /usr/bin/tomcatup
	- sudo ln -s /home/ec2-user/apache-tomcat-9.0.41/bin/shutdown.sh /usr/bin/tomcatdown
	- http://your_public_dns:8080/
	- configure Apache Tomcat Users (conf/tomcat-users.xml)
		- sudo vi tomcat-users.xml
	- allow all IPs (webapps/manager/META-INF/context.xml)
		- sudo vi context.xml

```html
<!-- manage tomcat users (conf/tomcat-users.xml) -->
<tomcat-users>
	<user username="manager_id" password="manager_pwd" roles="admin-gui,manager-gui" />
</tomcat-users>

<!-- allow all IPs (webapps/manager/META-INF/context.xml) -->
<Context>
	<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
</Context>
```

- Installing MySQL
	- sudo yum install mariadb-server
	- sudo systemctl start mariadb
	- sudo systemctl enable mariadb
	- sudo mysql_secure_installation
		- set root password (enter your new password)
	- create database
		- mysql -u root -p
		- show databases;
		- create database photo_app;
		- exit
	- create user
		- create user 'new_user_name'@'localhost' identified by 'new_user_pwd';
		- grant all privileges on photo_app.* to 'new_user_name'@'localhost';
		- flush privileges;
		- exit
		- mysql -u new_user_name -p
		- show databases;
		- use db_name;
		- show tables;
		- exit

### References
- [Apps Developer Blog](https://www.appsdeveloperblog.com/)
- [How to connect to EC2 with PuTTY (Windows)](https://www.youtube.com/watch?v=bi7ow5NGC-U&feature=youtu.be)
- [Connect EC2 Instance using Putty | MobaXTerm](https://www.youtube.com/watch?v=_d-c9uGcUrU)
- [Install MySQL server in Amazon Linux 2](https://www.youtube.com/watch?v=h6sdw6wWNbY)
- [Installing Tomcat 9 on EC2 server | Deploying your Java and WAR files](https://www.youtube.com/watch?v=DzLhABPzI90)
