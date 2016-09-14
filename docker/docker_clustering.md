## Docker Swarm
### Architecture
- Swarm master
- Swarm hosts

### Create a Swarm cluster
- Manually  
Ref: https://docs.docker.com/swarm/install-manual/  

```
# create the swarm master
docker run --rm swarm create

# start the swarm manage
docker run –d –P swarm manage token://<cluster token>

# connect a node to the cluster
# add –H 0.0.0.0:2375 to DOCKER_OPTS in /etc/default/docker and restart the docker service
docker run –d swarm join --addr=<node ip>:<daemon port> token://<cluster token>

# list nodes in the cluster
docker run --rm swarm list token://<cluster_id>

# connect the docker client to swarm
## option 1: Configure the DOCKER_HOST variable
export DOCKER_HOST=172.17.0.2:<swarm port>

## option 2: Run docker client and specify the daemon to connect to
docker –H tcp://172.17.0.2:<swarm port>

# verify the docker client
docker info
```

### [Create a Swarm using Docker Machine](https://docs.docker.com/swarm/install-w-machine/)
- Get a new token  
`docker run --rm swarm create`

- Create Swarm master
```
docker-machine create -d virtualbox \
    --swarm \
    --swarm-master \
    --swarm-discovery token://<token> \
    swarm-master
```

- Create Swarm nodes
```
docker-machine create -d virtualbox \
    --swarm \
    --swarm-discovery token://<token> \
    swarm-node-1
docker-machine create -d virtualbox \
    --swarm \
    --swarm-discovery token://<token> \
    swarm-node-2
```

- Connect to Swarm master
```  
evel $(docker-machine env --swarm swarm-master)
docker info
```

- Create containers
```
docker run -d -p 80:80 --name nginx1 nginx
docker run -d -p 80:80 --name nginx2 nginx
docker run -d -p 80:80 --name nginx3 nginx
docker ps
```


### Swarm Manager failover
Use the —replication and —advertise flags. This tells Swarm that there will be other managers for failover. It will also tell Swarm what address to advertise on, so the other managers know on what IP address to connect for other Swarm managers.


## Kubernetes
