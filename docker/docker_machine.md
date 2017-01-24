Docker Machine can be used to create Docker hosts.

## Installation
- Install [Docker Toolbox](https://www.docker.com/products/docker-toolbox)
- Install only Docker Machine:   
  https://docs.docker.com/machine/install-machine/  
  https://github.com/docker/machine/releases/


## Setting up hosts
```
docker-machine create --driver <driver> <machine_name>  

# examples:
# local vm
docker-machine create --driver virtualbox default

# cloud providers (https://docs.docker.com/machine/drivers/)
docker-machine create --driver digitalocean --digitalocean-access-token xxxxx docker-sandbox
docker-machine create --driver amazonec2 --amazonec2-access-key AKI******* --amazonec2-secret-key 8T93C*******  aws-sandbox
docker-machine create --driver amazonec2 --amazonec2-access-key <AWS_ACCESS_KEY> --amazonec2-secret-key <AWS_SECRET_KEY> --amazonec2-subnet-id east-1b amazonhost

# Add a host without a driver
docker-machine create --url=tcp://50.134.234.20:2376 custombox
```


## Host management
- Start/stop/restart a docker machine  
`docker-machine start <machine_name>`  
`docker-machine stop <machine_name>`  
`docker-machine restart <machine_name>`

- Get docker machine environment settings  
`docker-machine env <machine_name>`  
`docker-machine active`

- Get docker machine ip  
`docker-machine ip <machine_name>`   

- Destroy a docker machine  
`docker-machine rm <machine_name>`

- Ssh into a docker-machine  
`docker-machine ssh`



## Host monitoring
- List all docker machines  
`docker-machine ls`

- GUI tools  
[Shipyard](https://shipyard-project.com/)  
[DockerUI](https://github.com/crosbymichael/dockerui)  
[Panamax](http://panamax.io/)



## Port Forwarding with a Docker Machine
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
