### Config Server
- Create EC2 Instance
- connect
	- ssh -i "ms-photo-app-api-keypair.pem" ec2-user@your_ec2_public_dns
- install java
	- sudo yum update
	- java -version
	- yum list java*
	- sudo yum install java-1.8.0
	- java -version
- install docker
	- sudo amazon-linux-extras install docker
	- sudo service docker start
	- docker status
		- systemctl status docker.service
	- sudo usermod -a -G docker ec2-user
	- make docker auto-start
		- sudo chkconfig docker on
	- sudo reboot // Reboot to verify it all loads fine on its own

- Run RabbitMQ Docker Container
	- docker run -d --name rabbit-name-management -p 15672:15672 -p 5672:5672 -p 15671:15671 -p 5671:5671 -p 4369:4369 rabbitmq:3-management
	- docker ps
	- docker stop <container_id>
	- docker ps -a
	- docker start <container_id>
	- Edit inbound rules
	
| Type		| Protocol  | Port range  | Source     | Description - optional |
| -----------	| ----------| ------------| ---------  | ---------------------- |
| SSH		| TCP  	    | 22          | 0.0.0.0/0  |                        |
| Custom TCP	| TCP	    | 4369        | 0.0.0.0/0  | RabbitMQ port          |
| Custom TCP	| TCP	    | 4369        | ::/0       | RabbitMQ port          |
| Custom TCP	| TCP	    | 5672        | 0.0.0.0/0  | RabbitMQ port          |
| Custom TCP	| TCP	    | 5672        | ::/0       | RabbitMQ port          |
| Custom TCP    | TCP       | 15672       | 0.0.0.0/0  | RabbitMQ port          |
| Custom TCP	| TCP	    | 15672       | ::/0       | RabbitMQ port          |
| Custom TCP	| TCP	    | 5671        | 0.0.0.0/0  | RabbitMQ port          |
| Custom TCP	| TCP	    | 5671        | ::/0       | RabbitMQ port          |
| Custom TCP    | TCP       | 15671       | 0.0.0.0/0  | RabbitMQ port          |
| Custom TCP	| TCP	    | 15671       | ::/0       | RabbitMQ port          |

	- test: your_ec2_public_dns:15672 (guest/guest)
- To run RabbitMQ and change Default user name and password:
	- docker run -d --name rabbit-name-management -p 15672:15672 -p 5672:5672 -p 5671:5671 -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password rabbitmq:3-management

- Create docker file
```java
/* Dockerfile */
FROM openjdk:8-jdk-alpine 
VOLUME /tmp 
COPY apiEncryptionKey.jks apiEncryptionKey.jks
COPY UnlimitedJCEPolicyJDK8/* /usr/lib/jvm/java-1.8-openjdk/jre/lib/security/ 
COPY target/PhotoAppAPIConfigServer-0.0.1-SNAPSHOT.jar ConfigServer.jar 
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","ConfigServer.jar"] 
```

- Create docker image and publish the image to Docker Hub
	- mvn clean
	- mvn package
	- docker build --tag=config-server --force-rm=true .
	- docker images
	- docker login --username=your_dockerhub_username
	- docker tag <image_id> your_dockerhub_username/config-server
	- docker push your_dockerhub_username/config-server

- Run Config Server on EC2 from Docker Hub
	- connect your config server
	- docker ps
	- docker start <rabbitmq_container_id> // start RabbitMQ
	- docker ps
	- docker login --username=your_dockerhub_username
	- docker inspect <rabbitmq_container_id> // find ip_address
	- docker run -d -p 8012:8012 -e "spring.rabbitmq.host=rabbitmq_ip_address" your_dockerhub_username/config-server
	- docker ps
	- docker logs <container_id>
	- Edit inbound rules
		- EC2 Private IP: 172.31.10.143 => 172.31.0.0/24
| Type		| Protocol  	| Port range  	| Source     | Description - optional |
| -----------	| ----------	| ------------	| ---------  | ---------------------- |
| Custom TCP  	| TCP 		| 8012		| Private IP | Config Server          |
| Custom TCP	| TCP		| 8012		| My IP      | Config Server          |

	- test: ec2_public_dns:8012/application_name/default 
