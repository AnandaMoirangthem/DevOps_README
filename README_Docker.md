# Docker Instructions 

## Installation on Mac
To Install Docker on Mac, follow the instructions at below link.

https://docs.docker.com/docker-for-mac/install/

## Installation on Windows
To install Docker on Windows, follow the intructions at below link.

https://docs.docker.com/docker-for-windows/install/

## Installation on Linux Distribution
To install Docker on Linux, follow the instructions at below link.

Centos : https://docs.docker.com/engine/install/centos/

Ubantu : https://docs.docker.com/engine/install/ubuntu/

Debian : https://docs.docker.com/engine/install/debian/

Fedora : https://docs.docker.com/engine/install/fedora/


## Docker Hub
To access Docker Hub, Please visit below url

https://hub.docker.com/

## Docker Commands

> 1. To check Docker Version :
 ```bash
  docker --version
  
 ```
 
> 2. To Start the Docker on Linux : 
```bash
  sudo service docker start
  
 ```
 
> 3. To Stop the Docker on Linux : 
```bash
  sudo service docker stop
  
 ```
 
> 4. To check the running containers :
```bash
  docker ps
  
 ```
 
> 5. To check all the containers (running and terminated) :
```bash
  docker ps -a
  
 ```
 
> 6. To check docker images : 
```bash
  docker image
  
 ```
 
> 7. To remove an docker image :
```bash
  docker rmi $image_id
  
 ```
 
> 8. To build a docker image from Dockerfile : 
```bash
  docker build 
  docker build -t $imageName:tagName
  docker build -t my-test-app:1.0.0
  
 ```
 
 > 9. To Login to Docker Hub Repository : 
  ```bash
   docker login
   
 ```
 
 > 10. To Push an image to DockerHub Repository :
  ```bash
   docker push $dockerHubId/$imageId:$tagName
   docker push shubhamkushwah123/my-test-app:1.0.0
   
 ```
 
 > 11. To Pull an image from DockerHub Repository :
  ```bash
   docker pull $dockerHubId/$imageId:$tagName
   docker pull shubhamkushwah123/my-test-app:1.0.0
   
 ```
 
 > 12. To Run an Docker image :
  ```bash
   docker run -p $entryPort:$PortToBeMapped -d $dockerId/$imageId:$tagName
   docker run -p 8888:8080 -d shubhamkushwah123/my-test-app:1.0.0
   
 ```

## Dockerfile Keywords

> 1. FROM : It tells docker from which base image, you want to create your image from.
Usage : 
```bash
FROM <image>
FROM <image>:<tag>
```
> 2. RUN : This command is used to run the instructions against the image. 
Usage : 
```bash
RUN <command>
```
> 3. CMD : CMD is used to provide default arguments for the ENTRYPOINT instruction, both the CMD and ENTRYPOINT instructions should be specified with the JSON array format.
Note : Please note that there can only be one CMD in a Dockerfile.
Usage : 
```bash
CMD <command> <param1> <param2> (shell form)
```

> 4. MAINTAINER : It is used to specify the maintainer of the Dockerfile. In our case, we just specify the email id.
Usage : 
```bash
MAINTAINER <name>
```

> 5. ADD : Adds new files, directories, or remote file URLs from <src> and adds them to the filesystem of the image at the path <dest>.
Usage : 
 
```bash
ADD <src>  <dest>
```

> 6. ENTRYPOINT : Allows you to configure a container that will run as an executable.
Usage : 

```bash
ENTRYPOINT <command> <param1> <param2> (shell form)
```

> 7. ENV : The ENV instruction sets the environment variable <key> to the value <value>.
Usage : 
 
```bash
ENV <key> <value>
```

> 8. EXPOSE : It Informs Docker that the container listens on the specified network port(s) at runtime.
Usage : 

```bash
EXPOSE <port> [<port> ...]
```

> 9. USER : The USER instruction sets the user name or UID to use when running the image and for any RUN, CMD and ENTRYPOINT instructions that follow it in the Dockerfile.
Usage : 

```bash
USER <username | UID>
```

> 10. VOL : Creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.
Usage : 

```bash
VOLUME <path>
```

> 11. COPY : Copies new files or directories from <src> and adds them to the filesystem of the image at the path <dest>.
Usage : 
 
```bash
COPY <src> [<src> ...] <dest>
```

> 12. WORKDIR : Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD instructions that follow it.
Usage : 

```bash
WORKDIR </path/to/workdir>
```

> 13. ARG : Defines a variable that users can pass at build-time to the builder with the docker build command.
Usage : 

```bash
ARG <name>[=<default value>]
```



## Dockerfile Example

```bash
FROM openjdk:8-jdk-alpine

VOLUME /tmp

EXPOSE 8888

ARG JAR_FILE=/target/*.jar

COPY ${JAR_FILE} app.jar

RUN echo "Creation of your docker image is in progress, please hold on for a moment"

ENTRYPOINT ["java", "-jar", "app.jar"]

MAINTAINER "shubhamkushwah123@gmail.com"
```

# Docker Compose file : 
```
version: '3'
services:
  docker-mysql:
    restart: always
    container_name: docker-mysql
    image: mysql
    environment:
      MYSQL_DATABASE: book_manager
      MYSQL_ROOT_PASSWORD: root
      MYSQL_ROOT_HOST: '%'
    volumes:
      - ./sql:/docker-entrypoint-initdb.d
    ports:
      - "6033:3306"
    healthcheck:
      test: "/usr/bin/mysql --user=root --password=root--execute \"SHOW DATABASES;\""
      interval: 2s
      timeout: 20s
      retries: 10
  my-app:
    restart: on-failure
    build: ./
    ports:
    - 8888:8080
    environment:
      WAIT_HOSTS: mysql:3306
    depends_on:
    - docker-mysql
```

# Docker Mysql Container Connection :

https://phoenixnap.com/kb/mysql-docker-container

# Docker Networking : 

1. Every installation of the Docker Engine automatically includes three default networks. You can list them:

```
$ docker network ls

NETWORK ID          NAME                DRIVER
18a2866682b8        none                null
c288470c46f6        host                host
7b369448dccb        bridge              bridge
```


2. The network named bridge is a special network. Unless you tell it otherwise, Docker always launches your containers in this network. Try this now:
```
$ docker run -itd --name=networktest ubuntu

74695c9cea6d9810718fddadc01a727a5dd3ce6a69d09752239736c030599741
```

3. Inspecting the network is an easy way to find out the container’s IP address.
```
$ docker network inspect bridge
```

4. You can remove a container from a network by disconnecting the container. To do this, you supply both the network name and the container name. You can also use the container ID. In this example, though, the name is faster.
```
$ docker network disconnect bridge networktest
```

While you can disconnect a container from a network, you cannot remove the builtin bridge network named bridge. Networks are natural ways to isolate containers from other containers or other networks. So, as you get more experienced with Docker, create your own networks.


## Create your own bridge network
```
$ docker network create -d bridge my_bridge
```
The -d flag tells Docker to use the bridge driver for the new network. You could have left this flag off as bridge is the default value for this flag.

Use the docker network rm command to remove a user-defined bridge network. If containers are currently connected to the network, disconnect them first.
```
$ docker network rm my_bridge
```

## Connect a container to a user-defined bridge🔗

When you create a new container, you can specify one or more --network flags. This example connects a Nginx container to the my-net network. It also publishes port 80 in the container to port 8080 on the Docker host, so external clients can access that port. Any other container connected to the my-net network has access to all ports on the my-nginx container, and vice versa.
```
$ docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest
  ```
To connect a running container to an existing user-defined bridge, use the docker network connect command. The following command connects an already-running my-nginx container to an already-existing my-net network:
```
$ docker network connect my-net my-nginx
```
To disconnect a running container from a user-defined bridge, use the docker network disconnect command. The following command disconnects the my-nginx container from the my-net network.
```
$ docker network disconnect my-net my-nginx
```



