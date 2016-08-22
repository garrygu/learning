[Docker Toolbox](https://www.docker.com/docker-toolbox)
> Available for both Windows and Mac, the Toolbox installs Docker Client, Machine, Compose and Kitematic.

## Settings
**Proxy**
- NO_PROXY  
`export NO_PROXY=$(docker-machine ip default)`

- Print environmental variables  
`printenv | grep PROXY`

- Setup docker machine to use proxy  
```
  docker-machine ssh default  

  # get root access  
  sudo -s

  # configure the proxy
  echo "export HTTP_PROXY=http://username:password@corporate.proxy.com:port" >> /var/lib/boot2docker/profile
  echo "export HTTPS_PROXY=http://username:password@corporate.proxy.com:port" >> /var/lib/boot2docker/profile

  # restart the docker machine
  docker-machine restart default
```
