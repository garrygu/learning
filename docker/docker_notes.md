# Docker General Notes

## Basic Concepts
**Analogy with dev：**  
  image: class   
  layer: inheritance  
  container: instance


## Docker Commands
`docker help`
`docker COMMAND --help`
`docker version`


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

**Notes:**
  - Can assume a common port
  - Difficult to spin multiple images if images specify a port


### Other Image Commands
- List out all the images (on the host machine)  
`docker images`  

- Searching for image  
`docker search <image_name>`  

  "Stars" indicates the popularity of the images; "Official" images are those in the root namespace; "Automated" images are built automatically by the Docker Hub

- Viewing image history  
`docker history <image_name>`  

- Delete docker image  
`docker rmi [OPTIONS] IMAGE [IMAGE...]`  

  Options:  
  -f (--force): force removal of the image      

  Example:  
  `docker rmi -f nginx`


### Image Storage  
#### Docker Registry Hub  
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

#### The locally run Docker registry
Locally run by yourself to storage images




## Docker Container
- A Docker container is created when execute `docker run <image-name>`
- To optimize docker boot time, copy-on-write is used instead of regular copy
- Two states: **running** or **exited**


### Create a container
```
docker create [OPTIONS] IMAGE [COMMAND] [ARGS...]
```

### Run a command in a new container
```
docker run [OPTIONS] IMAGE [COMMAND] [ARGS...]
```

|Options|Description|
|---|---|
|-d (--detach)   |       Run container in background and print container ID|
|--attach       | Attach a running container
|-i (--interactive)  |   Keep STDIN open even if not attached|
|-t (--tty)  |  Allocate a pseudo-TTY|
|-P (--publish-all)  |   Publish all exposed ports to random ports|
|-p (--publish=[])   |   Publish a container's port(s) to the host|
|-v             |Mount a directory from host to container|


Examples
```
docker run -d -P nginx  
docker run -d -p 80:80 nginx
docker run -it --rm -p 8080-8081:8080-8081 mkobit/nifi
docker run -d -v $(pwd):/opt/namer -p 80:9292 training/namer
# [host-path]:[container-path]:[rw|ro]
# rw|ro: write status of the volume (rw by default)
```


### Container management
- Start a running container  
`docker start <name>`  

- shutdown a container gracefully
`docker stop <name>`

- kill a container immediately  
`docker kill <name>`    

- Re-connect to a container running in the background  
`dokcer attach <name>`

- Rename a container  
`docker rename <old_name> <new_name>`

- Remove a container  
`dokcer rm [-f] <name>`  
Example:  
`docker rm -f nginx1 nginx2 nginx3`

- Remove all containers  
`docker rm $(docker ps -qa)`  

- Remove all containers including running  
`docker rm -f $(docker ps -qa)`



### Container monitoring
- List all running containers  
`docker ps`

- List all containers, including stopped  
`docker ps -a`

- See the ID of the last container running  
`docker ps -lq`  

- Display a live stream of container(s) resource usage statistics  
`docker stats <container_name>`

- Display the running processes of a container  
`docker top <container_name>`

- Access everything written to the **STDOUT** from within a container  
`docker logs <container-id|name>`  

- Display the last line of the log  
`docker logs --tail 1`  

- Inspect docker container   
`docker inspect <containerID`  
`docker inspect <containerID | jq .`  # parsing JSON with the shell  
`docker inspect --format '{{ json .Created }}' <containerID>`  
`docker inspect --format '{{ .NetworkSettings.IPAddress}}' <containerID>`  
The expression starts with a dot representing the JSON object  
The optional json keyword asks for valid JSON output. (e.g. here it adds the surrounding double-quotes.)

- Get real time events from the server
`docker events`

- Find the port  
`docker port <containerID> 8000`

- Export a container's file system as a tar archive and send it to **STDOUT**  
`docker export <container-id|name>`  

- Copy files/folders between a container and the local file system  
`docker cp <container:path> <hostpath>`  
Use `docker cp` instead of `export` if only one directory or file is needed from a container


### Automatic restarts  
Ref: [Ensuring Containers Are Always Running with Docker's Restart Policy](https://blog.codeship.com/ensuring-containers-are-always-running-with-dockers-restart-policy/)  

- Restart policy to apply when a container exits (default "no")  
`docker run -d --restart=always ...`  

- Use docker update (from v1.11.0)  
`docker update --restart=always <CONTAINER ID>`  




## Networking/linking
How containers are networked or linked together

#### Network models
- Bridge
- None
- Host
- Container-to-Container

When starting a container, we have a few modes to select its networking:  
* --net=bridge: This is the default mode.
`docker run -i -t --net=bridge centos /bin/bash`

* --net=host: Docker does not create a network namespace for the container; instead, the container will network stack with the host.
`docker run -i -t --net=host centos bash`


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




## Security
http://www.slideshare.net/jpetazzo/docker-linux-containers-lxc-and-security

### Secure docker with TLS

### Security best practices
- Run Docker containers or attach containers to Docker volumes using the read-only modes  
`docker run -d -v /opt/uploads:ro nginx`  
`docker run -d —volumes-from data:ro nginx`  
- Utilize the Docker security benchmark application
- Utilize the Docker command-line tools to see what has changed in a particular image  
`docker diff`  
`docker inspect`  
`docker history`  



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



## Tools
### [DockerUI](https://github.com/kevana/ui-for-docker)
> UI For Docker is a web interface for the Docker Remote API. The goal is to provide a pure client side implementation so it is effortless to connect and manage docker.

Installation:  
`$ docker run -d -p 9000:9000 --privileged -v /var/run/docker.sock:/var/run/docker.sock dockerui/dockerui`

### [ImageLayers](https://imagelayers.io)
> Visualize Docker Images and the layers that compose them
