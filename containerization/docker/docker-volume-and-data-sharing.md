# Docker: Volume and Data Sharing


## Volume can be declared in two ways:
- Within a Dockerfile, with a VOLUME instruction  
`VOLUME /var/lib/postgres `

- Use the -v flag for docker run  
```
docker run -d -v /var/lib/postgres training/postgres
docker run -it -v /:/hostfs -w /hostfs ubuntu bash
```
You can use multiple -v directives.  
:ro is the read-only flag


## Share volumes across containers
**Data container**  
Container created for the sole purpose of referencing one (or many) volumes.  

Example 1:
```
docker run --name wwwdata -v /var/lib/www busybox true
docker run --name wwwlogs -v /var/log/www busybox true

docker run -d --volumes-from wwwdata --volumes-from wwwlogs webserver
docker run -d --volumes-from wwwdata ftpserver
docker run -d --volumes-from wwwlogs logstash
```

Example 2:
```
# from terminal 1
docker run -it --name alpha -v /var/log ubuntu bash

# from terminal 2
docker run --volumes-from alpha ubuntu cat /var/log/now
```

Data volume containers are special; they can be stopped and still fulfill their purpose.   
Keep a data container running example:  
`docker run –d <image_name> tail –f /dev/null`


## Backup and restore data volumes
Create a .zip file (from your host) from the data inside a data volume container that has VOLUME ["/var/www"] in its Dockerfile (by mounting data onto a temporarily container):  
`docker run --volumes-from data-container -v $(pwd):/host ubuntu zip -r /host/data-containers-www /var/www`


## Share a socket  
`docker run -it -v /var/run/docker.sock:/docker.sock ubuntu bash `


## Volume Inspection
Check volumes defined by an image  
`docker inspect <image_name>`

Check volumes used by a container  
`docker inspect <containerID>`



## More info
- https://docs.docker.com/engine/userguide/containers/dockervolumes/ 
- https://docs.docker.com/engine/reference/builder/#volume
