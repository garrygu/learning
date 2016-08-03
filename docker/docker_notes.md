# Docker Notes

## Docker Components
- Docker image
- Docker container
- Docker CLI
- Docker registry hub

## Docker Image
- Base image / Parent image - A docker image can build on top of another image, and so on. The first image layer is called a **base image**; all images layers except the last image layer are called **parent images**
- Image ID - a 64-character long hexadecimal string
### Docker Image Commands
|cmd|description|
|---|---|
|docker images|
|docker pull <image-name>[:tag]
### Dockerfile


## Docker Container
A Docker container is created when execute **docker run <image-name>**  
Two states: **running** or **exited**

## Docker CLI
* docker logs <container-id|name>  
Access everything written to the **STDOUT** from within a container
* docker export <container-id|name>  
Archive a container data (tar) and send it to **STDOUT**
* docker cp <container:path> <hostpath>  
Use **docker cp** instead of **export** if only one directory or file is needed from a container

## Docker Registry Hub
https://hub.docker.com/ - Where you can share, find and extend docker images
