### Config Server
- Create EC2 Instance
- connect
	- ssh -i "ms-photo-app-api-keypair.pem" ec2-user@ec2-13-236-191-96.ap-southeast-2.compute.amazonaws.com
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

- Run RabbitMQ Docker Container
	- docker run -d --name rabbit-name-management -p 15672:15672 -p 5672:5672 -p 15671:15671 -p 5671:5671 -p 4369:4369 rabbitmq:3-management
	- docker ps
	- test
		- ec2-13-236-191-96.ap-southeast-2.compute.amazonaws.com:15672 (guest/guest)
