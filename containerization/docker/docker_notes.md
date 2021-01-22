# Docker General Notes
- Write using Go
- Namespaces isolate process, network, etc.
- Cgroups manage resource allocation
- Union File System combine separate file systems to logically form a union file system
- Copy on Write for containers & images

## Basic Concepts
**Analogy with dev：**  
  image: class   
  layer: inheritance  
  container: instance

Â
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
`docker search --filter=stars=3 <image_name>`

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


Examples:
```
docker run -d -P nginx  
docker run -d -p 80:80 nginx
docker run -it --rm -p 8080-8081:8080-8081 mkobit/nifi
docker run -d -v $(pwd):/opt/namer -p 80:9292 training/namer
# [host-path]:[container-path]:[rw|ro]
# rw|ro: write status of the volume (rw by default)
```


### Passing parameters to containers
#### Baking the Configuration into the Container  
You can have a setup script as `ENTRYPOINT` or a foreground script configuring container settings.   

Dockerfile example:  
```
FROM debian:latest
ADD ./case.sh /root/case.sh
RUN chmod +x /root/case.sh
ENTRYPOINT /root/case.sh
```

#### Via Environment Variables  
Can pass environment variables to containers with the -e flag.  

```
docker run --name mysql-one -e MYSQL_ROOT_PASSWORD=pw -d mysql

#-e can pull in the value from the current environment if you just give it without the =
docker run --name mysql-one -e MYSQL_ROOT_PASSWORD -d mysql
```
>The container’s entry point (startup script) will look for the environment variables, and “sed” or “echo” them into  relevant configuration files the application uses before actually starting the app.  
The container’s entry point script should contain reasonable defaults for each of the environment variables if the invoker does not pass those environment variables in.

**For more complex configurations**  
The container’s startup script will reach out to a key-value (KV) store on the network like Consul or etcd to get configuration parameters.


#### Map the Config Files in Directly via Docker Volumes
`docker run -v /home/dan/my_statsd_config.conf:/etc/statsd.conf hopsoft/graphite-statsd`


Reference:  
[How Should I Get Application Configuration into my Docker Containers?](https://dantehranian.wordpress.com/2015/03/25/how-should-i-get-application-configuration-into-my-docker-containers/)


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
Remove all volumes on file system:  
`docker rm -v $(docker ps -a -q)`  


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
`docker inspect <containerID>`  
`docker inspect <containerID> | jq .`  # parsing JSON with the shell  
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


### Metadata & Labels
- Add a single label  
`docker run -l user=12345 -d redis`  
- Add multiple labels using external file (The file needs to have a label on each line)  
`docker run --label-file=labels -d redis`

- Add a single label to images  
`LABEL vendor=Katacoda`
- Add multiple labels to images  
```
LABEL vendor=Katacoda   
 \ com.katacoda.version=0.0.5   
 \ com.katacoda.build-date=2016-07-01T10:47:29Z
 \ com.katacoda.course=Docker
 ```

- Inspect image   
`docker inspect -f "{{json .Config.Labels }}" rd`
- Inspect container   
`docker inspect -f "{{json .ContainerConfig.Labels }}" <containerID>`

- Filter containers  
`docker ps --filter "label=user=scrapbook"`
- Filer images  
`docker images --filter "label=vendor=Katacoda"`
- Daemon labels  
```
docker -d \
  -H unix:///var/run/docker.sock \
  --label com.katacoda.environment="production" \
  --label com.katacoda.storage="ssd"
```


### Automatic restarts  
Ref: [Ensuring Containers Are Always Running with Docker's Restart Policy](https://blog.codeship.com/ensuring-containers-are-always-running-with-dockers-restart-policy/)  

- Restart policy to apply when a container exits (default "no")  
`docker run -d --restart=always ...`  

- Use docker update (from v1.11.0)  
`docker update --restart=always <CONTAINER ID>`  



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
