# Docker Cheat Sheet

## Docker
- list all containers

´> docker ps -a´

- list all local images

`> docker image ls`

- create child image 

`> docker commit "old image" "new child image"`

- start container (custom name, expose port to host)

`> docker run --name "name" -p hostport:containerport repo/image:tag`

- remove container

`docker rm containerid/name`

## Docker-Compose


## Docker Swarm