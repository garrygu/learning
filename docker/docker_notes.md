# Docker Notes

## Docker Components
- Docker image
- Docker container
- Docker CLI
- Docker registry hub

## Docker Image
- Base image / Parent image - A docker image can build on top of another image, and so on. The first image layer is called a **base image**; all images layers except the last image layer are called **parent images**
- Image ID - a 64-character long hexadecimal string
### Commands
|cmd|description|
|---|---|
|docker images|
|docker pull <image-name>[:tag]

### Notes
- Can assume a common port
- Difficult to spin multiple images if images specify a port

### Create Docker Images
#### Create docker images using Dockerfile

## Docker Container
A Docker container is created when execute **docker run <image-name>**  
Two states: **running** or **exited**

### Commands
##### Run a command in a new container
$docker run [OPTIONS] IMAGE [COMMAND] [ARGS...]  

|Options|Description|
|---|---|
|-d (--detach)   |       Run container in background and print container ID|
|-i (--interactive)  |   Keep STDIN open even if not attached|
|-t (--tty)  |  Allocate a pseudo-TTY|
|-P (--publish-all)  |   Publish all exposed ports to random ports|
|-p (--publish=[])   |   Publish a container's port(s) to the host|
**Examples**
> $docker run -d -P nginx  
> $docker run -d -p 80:80 nginx

##### Delete an image
$docker rmi [OPTIONS] IMAGE [IMAGE...]
|Options|Description|
|---|---|
|-f (--force)   |   Force removal of the image|
**Examples**
> $docker rmi -f nginx

## Docker CLI
* docker logs <container-id|name>  
Access everything written to the **STDOUT** from within a container
* docker export <container-id|name>  
Archive a container data (tar) and send it to **STDOUT**
* docker cp <container:path> <hostpath>  
Use **docker cp** instead of **export** if only one directory or file is needed from a container

## Docker Registry Hub
https://hub.docker.com/ - Where you can share, find and extend docker images

## Tools for Docker Orchestration
### Docker Machine
### Docker Compose
### Docker Swarm
### Kubernetes

##  For MacOS
