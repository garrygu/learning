## Overview
- Based on an older orchastration tool called [Fig](http://www.fig.sh)  
- Allow you to define how applications are structured into Docker containers while bundling everything up into an easy-to-deploy configuration


## Commands
- Build a project  
`docker-compose up -d`

- Show container info for project  
`docker-compose ps`

- Stop a project  
`dokcer-compose stop`  
`dokcer-compose kill`

- Destroy all the containers  
`dokcer-compose rm`

- Add/Remove containers for scaling  
`docker-compose scale [container]=[size]`


## docker-compose.yml
[Compose file reference](http://docs.docker.com/compose/compose-file/)
