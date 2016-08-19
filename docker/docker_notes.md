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
|docker pull <image_name>[:tag]

### Notes
- Can assume a common port
- Difficult to spin multiple images if images specify a port

### Create Docker Images
- Via running a Container
- Via using a Dockerfile
```
docker build -t <image_name> .
```

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
> $docker run -it --rm -p 8080-8081:8080-8081 mkobit/nifi

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


## Docker Machine
- `docker-machine ls`                  # list all docker machines
- `docker-machine env <machine_name>`  # get docker machine environment settings
- `docker-machine ip <machine_name>`   # get docker machine ip
- `docker-machine ssh`                 # ssh onto a docker-machine

### Port Forwarding with a Docker Machine
**Use SSH**
```
# machine name: default

# http://blog.ssanj.net/posts/2015-12-06-port-forwarding-with-a-docker-machine.html
ssh -i ~/.docker/machines/default/id_rsa docker@$(docker-machine ip default) -N -L 8080:localhost:8080`

# simple way
# http://stackoverflow.com/questions/32174560/port-forwarding-in-docker-machine
docker-machine ssh default -L 8080:localhost:8080

# forwrd ports in abackground process
# -f Requests ssh to go to background just before command execution.
# -N Allow empty command (useful here to forward ports only)
docker-machine ssh default -f -N -L 8080:localhost:8080
```

**[Use VirtualBox port forwarding](http://stackoverflow.com/questions/35372399/connect-to-docker-machine-using-localhost)**
```
# map port 80 in container to localhost:8888
# use modifyvm if the vm isn't started yet
vboxmanage controlvm default --natpf1 "nameformapping,tcp,,8888,,80"

# delete existing rule
VBoxManage controlvm "default" natpf1 delete "${port_type}-port${port_num}"
```
Or through VirtualBox UI:  
https://www.virtualbox.org/manual/ch06.html#network_nat

**[Use Nginx](http://stackoverflow.com/questions/25327012/access-docker-from-external-machine-in-network)**
```
# /etc/nginx/sites-enabled/your-file.conf
server {                                                                   
    listen 3000;                                                              

    server_name YOUR_IP_ADDRESS;                                              

    proxy_redirect off;                                                       
    proxy_buffering off;                                                      
    proxy_set_header Host $host;                                              
    proxy_set_header X-Real-IP $remote_addr;                                  
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;              

    location / {                                                              
        proxy_pass http://127.0.0.1:3000;                                            
    }                                                                         
}
```
