# Docker: Networking/linking
How containers are networked or linked together

### Network models
- Bridge
- None
- Host
- Container-to-Container

When starting a container, we have a few modes to select its networking:  
* --net=bridge: This is the default mode.  
`docker run -i -t --net=bridge centos /bin/bash`

* --net=host: Docker does not create a network namespace for the container; instead, the container will network stack with the host.  
`docker run -i -t --net=host centos bash`

* --net=container:NAME_or_ID: Docker does not create a new network namespace while starting the container but shares it from another container.   
`docker run -i -t --name=centos centos bash`
`docker run -i -t --net=container:centos ubuntu bash`

* --net=none: Docker creates the network namespace inside the container but does not configure networking.  



### Network commands
`docker network --help`  
More info:
- https://docs.docker.com/engine/userguide/networking/work-with-networks/
- https://docs.docker.com/engine/userguide/networking/dockernetworks/
- http://www.slideshare.net/bbsali0/docker-networking-with-new-ipvlan-and-macvlan-drivers



### Connecting Containers
Linking relies on the naming of containers.  

Option:  
`--link`: takes the **name:alias** argument

Example 1:
```
docker run -d --name mycache redis

docker run -it --link mycache:redis nathanleclaire/redisonrails rails console
# The –link flag connects one container to another (in the format name:alias)

ping redis
# Links also provides a DNS entry corresponding to the name of the container
```

Example 2:
```
docker run -d -it --name centos_server centos /bin/bash
docker inspect --format='{{.NetworkSettings.IPAddress}}' <docker_id>

docker run -it --link centos_server:server --name client fedora /bin/bash

# in container
cat /etc/hosts
# An entry of the first container (server) is added to the /etc/hosts file in the client container;
# An environmental variable SERVER_NAME is set within the client
```



### Accessing containers from outside
* -P option  
Automatically maps any network port of the container to a random high port of the Docker host:  
`docker run --expose 80 -i -d -P --name f20 fedora /bin/bash`  
Example: 0.0.0.0:32768->80/tcp

* -p option  
All requests coming on port 5000 from any interface on the Docker host will be forwarded to port 22 of the centos2 container:  
`docker run -i -d -p 5000:22 --name centos2 centos /bin/bash`  
Bind to a specific interface:    
`docker run -i -d -p 192.168.1.10:5000:22 --name f20 fedora /bin/bash`  
Map port 22 of the container to the dynamic port of the host:    
`docker run -i -d -p 192.168.1.10::22 --name f20 fedora /bin/bash`  
Bind multiple ports:   
`docker run -d -i -p 5000:22 -p 8080:80 --name f20 fedora /bin/bash`  

Look up the public-facing port:  
`docker port f20 80`

look at all the network settings of a container:  
`docker inspect -f "{{ .NetworkSettings }}" f20`


### Create a Docker Overlay Network
https://learning.oreilly.com/scenarios/create-a-docker/9781492062660/  
Overlay networks allow containers to communicate as if they're on the same host. Under the covers they use VxLan features of the Linux Kernel.  

- Initialize swarm mode  
`docker swarm init`
- Join swarm mode  
`docker swarm join --token SWMTKN-1-4ecfp982nyzlur7sjtb9da09okx3bksaw1oktcgq0i3fcfdpd0-85702pqdvaykoful1qd9rqxem 172.17.0.23:2377`  
or  
`token=$(ssh -o StrictHostKeyChecking=no 172.17.0.23 "docker swarm join-token -q worker") && docker swarm join 172.17.0.23:2377 --token $token`  
- Create the overlay network
`docker network create -d overlay app1-network`  
`docker network ls`
- Deploy backend
`docker service create --name redis --network app1-network redis:alpine`  
`docker service ls`
- Deploy frontend
`docker service create \
    --network app1-network -p 80:3000 \
    --replicas 1 --name app1-web \
    katacoda/redis-node-docker-example`  
`docker ps`  
`curl host01`


### References
- [Docker Networking 101 – The defaults](http://www.dasblinkenlichten.com/docker-networking-101/)
