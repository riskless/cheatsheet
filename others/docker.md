
### Install Docker on EC2 Linux Instance
- sudo yum install docker 
- sudo service docker start 
	- Start Docker as Service
- sudo usermod -a -G docker ec2-user 
	- Make Docker Not Require sudo Anymore

### Docker Commands
- List Currently Running Docker Containers
	- docker ps -all
- Start Docker Container
	- docker start <CONTAINER_ID>
- Stop Running Docker Container
	- docker stop <CONTAINER_ID>
- Delete Docker Container
	- docker rm <CONTAINER_ID>
	- To delete a running Docker container, you will need to first stop it
- List Docker Images on Computer
	- docker images -a
- Remove Docker Image
	- docker rmi <IMAGE_ID>
	- docker rmi -f <IMAGE_ID>
- Remove All Docker Images on Computer
	- docker rmi $(docker images -f dangling=true)
- Build Docker Image
	- docker build --tag=albums-microservice --force-rm=true .
- Run Docker Image in Docker Container
	- docker run -d <IMAGE_NAME>
	- Where -d is used to detach the process so you can continue working with a terminal window and execute other commands.
- Check Docker Container Logs
	- docker logs <CONTAINER_ID>
	- docker logs ceaf9e1ebef5
- Inspect Docker Container
	- docker inspect <CONTAINER_ID>
- Execute Commands in Docker Container
	- docker exec -it <CONTAINER_ID> <COMMAND_TO_RUN>
	- docker exec -it 67e36143717b ls
	- docker exec -it 67e36143717b bash
- Pass Environment Variables
	- docker run <CONTAINER_ID> -e "<ENVIRONMENT_VARIABLE_NAME>=<VALUE>"
	- docker run ceaf9e1ebef5 -e "SPRING_PROFILES_ACTIVE=dev" -e "server.port=8080"
- List Docker Networks on Computer
	- docker network ls
- Create Custom Docker Bridge Network
	- docker network create --driver bridge <NEW_NETWORK_NAME>
	- then you can run Docker container in a newly created custom bridge network
		- docker run <CONTAINER_ID > --network <NAME_OF_CREATED_NETWORK>
- Publish Docker Image to Docker Hub (To publish an existing Docker image to a Docker Hub Repository)
	- Open Browser window and login to docker hub,
	- Create a new Repository in your Docker Hub account,
	- Open a Terminal window on your computer and Login to Docker Hub account:
		- docker login --username=<DOCKER HUB USER NAME>
	- List existing Docker images on your computer and look up Docker image ID of the Image you want to publish on Docker Hub.
		- docker images ls
	- Tag Docker image on your computer with a Docker Hub repository name the following way:
		- docker tag <CONTAINER ID> <DOCKER HUB USERNAME>/<REPOSITORY NAME>
		- docker tag ceaf9e1ebef5 haroldkim/albums-microservice
	- Push Docker image on your computer to a Docker Hub Repository name:
		- docker push <Docker Hub User name>/<Repository name>
		- docker push haroldkim/albums-microservice
- Bind a Directory in Docker Container to a Directory on Host Machine
	- docker run -d -v esdata1:/usr/share/elasticsearch/data --name elasticsearch --network host docker.elastic.co/elasticsearch/elasticsearch:7.2.0
	- where: -v <directory on HOST machine>:<directory in Docker container>
- Make Docker Container use Host Network and Avoid Port Binding
	- docker run <IMAGE ID> --network host
### Dockerize Spring Boot App using Maven Plugin 
```java
/* pom.xml */
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>

			<plugin>
				<groupId>com.spotify</groupId>
				<artifactId>dockerfile-maven-plugin</artifactId>
				<version>1.4.3</version>
				<executions>
					<execution>
						<id>default</id>
						<goals>
							<goal>build</goal>
							<goal>push</goal>
						</goals>
					</execution>
				</executions>
				<configuration>
					<repository>repostiory-name/image-name</repository>
					<tag>${project.version}</tag>
				</configuration>
				<dependencies>
					<!-- To make this work on JDK 9+ -->
					<dependency>
						<groupId>javax.activation</groupId>
						<artifactId>javax.activation-api</artifactId>
						<version>1.2.0</version>
					</dependency>
				</dependencies>
			</plugin>

		</plugins>
</build>

/* Dockerfile */
FROM openjdk:11
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]

/* maven command */
mvn clean package dockerfile:push
```
### References
- [Docker Commands Cheat Sheet](http://appsdeveloperblog.com/docker-commands-cheat-sheet/)
- [Docker Tutorial for Beginners](https://www.youtube.com/watch?v=3c-iBn73dDE)
