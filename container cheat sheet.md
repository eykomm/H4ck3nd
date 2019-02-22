# Container Cheat Sheet
Some useful commands i found helpful while playing around with containers

## Docker

### Container

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

- copy a file from host to the container

`> docker cp "file" "container_id:/path/"`

- map data from another container

`> docker run ... --volumes-from "other_cont_name"`

- export a container

`> docker export "container_name" > "backup_cont.tar"`

- import an exported container

`> docker import "backup_cont.tar"`

- link two container

`> docker run ... --link "source_cont:alias"`

- link a data volume from the hostport read only (ro)

`> docker run ... -v "/host/path:/container/path:ro"`

- show used resoources by running container
`> docker stats --no-stream`

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

- attach a new network to a existing containers with an alias

`> docker network connect --alias "alias" "net_name" "container_name"`

- disconnect a container from a network

`> docker network disconnect "net_name" "container_name"`


### Dockerfile

- define master image

`FROM image_name:tag`

- run a command

`RUN "command"`

- expose certain port to host_port

`EXPOSE "port"`

- run a specific command

`CMD ["command", "argument"]`

## Docker Compose




## Docker Swarm




## Kubernetes (K8s)

- show cluster info

`> kubectl cluster-info`

- show active nodes

`> kubectl get nodes`

- show running pods

`> kubectl get pods`


### K8s Deployment

- deploy a container in the cluster with 2 replicas running at the same time

`> kubectl run "depl_name" --image="repo/image" --port="port" --replicas=2`

- expose a dynamic port on a running deployment

`> kubectl expose deployment "depl_name" --port="port" --type=NodePort `

- get all pods for a specific namespace

`> kubectl get pod -n "namespace"`


### K8s Multinode cluster

- start master node with known tokens

`> kubeadm init --token="token" --Kubernetes-version $(kubeadm version -o short)`

- list known tokens

`> kubeadm token list`

- get a node to join the cluster

`> kubeadm join --discovery-token-unsafe-skip-ca-verification --token="token" "ip_master:6443"`

- get all connected nodes (on master)

`> kubectl get nodes`


### K8s Network

- deploy a WaveWorks Addon for the Container Network Interface (CNI)

`> kubectl apply -f "waveworks.config"`

- use proxy instead of ip

`> kubectl proxy`

### K8s Dashboard

## Projects

### ELK Stack with Metricbeat for Monitoring

- Pull images

`> docker pull docker.elastic.com/elasticsearch/elasticsearch:6.6.1`
`> docker pull docker.elastic.com/kibana/kibana:6.6.1`
`> docker pull docker.elastic.com/beats/metricbeats:6.6.1`

- Create network

`> docker network create elastic`

- Create Metric Beat yml
--> see github for yml

- Start container

`> docker run --detach --net=elastic --name=elasticsearch -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 docker.elastic.co/elasticsearch/elasticsearch:6.6.1`

`> docker run --detach --net=elastic --name=kibana -p 5601:5601 docker.elastic.co/kibana/kibana:6.6.1`

`> docker run --detach --net=elastic --name=metric -u root -v /Users/lars/Hackspace/Docker/metricbeat/metricbeat.yml:/usr/share/metricbeat/metricbeat.yml -v /var/run/docker.sock:/var/run/docker.sock docker.elastic.co/beats/metricbeat:6.6.1`
