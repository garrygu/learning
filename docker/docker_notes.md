# Docker Notes

## Docker Components
- Docker image
- Docker container
- Docker CLI
- Docker registry hub

**Analogy with dev：**  
image: class   
layer: inheritance  
container: instance


## Docker Image
- An image is a read-only file system
- **Base image / Parent image** - Images are made of layers. The first image layer is called a `base image`; all images layers except the last image layer are called `parent images`
- **Image ID** - a 64-character long hexadecimal string
- Images can have tags (tags define image variants)


### Image Namespaces
- Root-like
- User (and organizations)
- Self-hosted


### Download Images
- Explicitly  
  `docker pull <image_name>[:tag]`
- Implicitly  
  `docker run` (when image is not found locally)


### Create Images
- Via running a Container  
  `docker commit`

- Via using a Dockerfile  
  `docker build -t <image_name> .`  
  `docker build --no-cache -t <image_name> .` # Do not use cache when building the image

- `docker import`

- Notes
  - Can assume a common port
  - Difficult to spin multiple images if images specify a port


### Other Image Commands
- `docker images`  
  Showing current images

- `docker search <image_name>`  
  Searching for image  
  "Stars" indicates the popularity of the images; "Official" images are those in the root namespace; "Automated" images are built automatically by the Docker Hub

- `docker history <image_name>`  
  Viewing image history

- `docker rmi [OPTIONS] IMAGE [IMAGE...]`  
  Delete docker image  

  Options:  
  -f (--force): force removal of the image      

  Example:  
  `docker rmi -f nginx`



## Docker Container
- A Docker container is created when execute `docker run <image-name>`
- To optimize docker boot time, copy-on-write is used instead of regular copy
- Two states: **running** or **exited**

### Commands
##### Run a command in a new container
$docker run [OPTIONS] IMAGE [COMMAND] [ARGS...]  

|Options|Description|
|---|---|
|-d (--detach)   |       Run container in background and print container ID|
|--attach       | Attach a running container
|-i (--interactive)  |   Keep STDIN open even if not attached|
|-t (--tty)  |  Allocate a pseudo-TTY|
|-P (--publish-all)  |   Publish all exposed ports to random ports|
|-p (--publish=[])   |   Publish a container's port(s) to the host|
|-v             |Mount a directory from host to container|
**Examples**  
```
docker run -d -P nginx  
docker run -d -p 80:80 nginx
docker run -it --rm -p 8080-8081:8080-8081 mkobit/nifi
docker run -d -v $(pwd):/opt/namer -p 80:9292 training/namer
# [host-path]:[container-path]:[rw|ro]
# rw|ro: write status of the volume (rw by default)
```

## Network
#### Network models
- Bridge
- None
- Host
- Container-to-Container

#### Network commands
`docker network --help`  
More info:
- https://docs.docker.com/engine/userguide/networking/work-with-networks/
- https://docs.docker.com/engine/userguide/networking/dockernetworks/
- http://www.slideshare.net/bbsali0/docker-networking-with-new-ipvlan-and-macvlan-drivers


#### Connecting Containers
```
docker run -d --name mycache redis

docker run -it --link mycache:redis nathanleclaire/redisonrails rails console
# The –link flag connects one container to another (in the format name:alias)

ping redis
# Links also provides a DNS entry corresponding to the name of the container
```

## Volume
Can be declared in two ways:
- Within a Dockerfile, with a VOLUME instruction  
`VOLUME /var/lib/postgres `

- Use the -v flag for docker run  
```
docker run -d -v /var/lib/postgres training/postgres
ddocker run -it -v /:/hostfs -w /hostfs ubuntu bash
```

Share volumes across containers:
```
# from terminal 1
docker run -it --name alpha - /var/log ubuntu bash

# from terminal 2
docker run --volumes-from alpha ubuntu cat /var/log/now
```

Share a socket
`docker run -it -v /var/run/docker.sock:/docker.sock ubuntu bash `

Data container  
Container created for the sole purpose of referencing one (or many) volumes.
```
docker run --name wwwdata -v /var/lib/www busybox true
docker run --name wwwlogs -v /var/log/www busybox true

docker run -d --volumes-from wwwdata --volumes-from wwwlogs webserver
docker run -d --volumes-from wwwdata ftpserver
docker run -d --volumes-from wwwlogs logstash
```

Check volumes defined by an image
`docker inspect <image_name>`

Check volumes used by a container
`docker inspect <containerID>`


More info:
- https://docs.docker.com/engine/userguide/containers/dockervolumes/ 
- https://docs.docker.com/engine/reference/builder/#volume  



## Docker CLI
- `docker ps -a`  
List all containers, including stopped
- `docker ps -lq`  
See the ID of the last container running

- `docker logs <container-id|name>`  
Access everything written to the **STDOUT** from within a container
- `docker logs --tail 1`  
See the last line of the log

- `docker export <container-id|name>`  
Archive a container data (tar) and send it to **STDOUT**

- `docker cp <container:path> <hostpath>`  
Use **docker cp** instead of **export** if only one directory or file is needed from a container

- Inspect docker container   
  `docker inspect <containerID`  
  `docker inspect <containerID | jq .`  # parsing JSON with the shell  
  `docker inspect --format '{{ json .Created }}' <containerID> `
    - The expression starts with a dot representing the JSON object
    - The optional json keyword asks for valid JSON output. (e.g. here it adds the surrounding double-quotes.)


- `docker port <containerID> 8000`
Find the port
- `docker inspect --format '{{ .NetworkSettings.IPAddress}}' <containerID`

- Remove container
`docker rm <containerID`
- Remove all containers
`docker rm $(docker ps -qa)`
- Remove all containers including running
`docker rm -f $(docker ps -qa)`



## Docker Registry Hub
https://hub.docker.com/ - Where you can share, find and extend docker images

- Login to Docker Hub  
`docker login`  
Credentials will be stored in ~/.docker/config.json

- Pulling images  
`docker pull ubuntu:14.04`

- Pushing images
  - Name image properly  
  `docker tag ipinfo garrygu/ipinfo`
  - Login
  - Push image  
  `docker push garrygu/ipinfo`

## Tools for Docker Orchestration
### Docker Machine
### Docker Compose
### Docker Swarm
### Kubernetes


## Security
http://www.slideshare.net/jpetazzo/docker-linux-containers-lxc-and-security

### Secure docker with TLS


## Docker API
The API binds locally to `unix:///var/run/docker.sock` but can also be bound to a network interface.



## Docker Daemon
- Running as a service  
`sudo service docker start/stop/restart`  

- Running the daemon interactively  
`sudo docker daemon [options] &`

- Start up options  
- Logging  
  - parameter: `--log-level`
  - options: Debug /Info /Warn /Error /Fatal  
  `sudo docker daemon --log-level=debug`
