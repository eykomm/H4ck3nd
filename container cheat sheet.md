# Container Cheat Sheet
Some useful commands i found helpful while playing around with containers

## Docker

#### Container

- list all containers

`> docker ps -a`

- start container (custom name, expose port to host)

`> docker run --name "name" -p hostport:containerport repo/image:tag`

- remove container

`> docker rm containerid/name`

- start container in background mode (detached) interactive and tty (see in and out)

`> docker run -dit --name "container_name" -p host_port:container_port "image_name" /bin/bash`

- connect to container running in background mode

`> docker attach "container_name"`

- detach from container without stopping

`CTRL p+q`

- start container in specific network

`> docker run --network "net_name" "image"`

- execute command in running container

`> docker execute "container_name" "command"`


### Images

- list all local images

`> docker image ls`

- create child image

`> docker commit "container_name" "new child image"`

- remove image

`> docker rmi "image_name"`


### Network

- list current networks

`> docker network ls`

- inspect network settings and connected containers

`> docker network inspect "network"`

- create custom bridged networks

`> docker network create --driver bridge "bridge_net_name"`

- remove custom network

`> docker network rm "net_name"`

- expose container to host networks

`> docker run --network host "image"`


### Dockerfile

- define master image

`FROM image_name:tag`

- run a command

`RUN "command"`

- expose certain port to host_port

`EXPOSE "port"`

- run a specific command

`CMD ["command", "argument"]`

## Docker-Compose




## Docker Swarm




# Kubernetes
