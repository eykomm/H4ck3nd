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


### Images

- list all local images

`> docker image ls`

- create child image

`> docker commit "old image" "new child image"`

- remove image

`> docker rmi "image_name"`


### Network

- list current networks

`> docker network ls`

- inspect network settings and connected containers

`> docker network inspect "network"`

---

## Docker-Compose


---

## Docker Swarm


---

# Kubernetes
