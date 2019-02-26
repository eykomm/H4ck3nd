# Kubernetes@Cloudhive

Documentation of creating a public K8S cluster

## Prequisites

### Nodes
physical / virtual machines running in a K8S cluster

### Worker

### Pods

Smallest unit in k8s. Containers are wrapped in pods.

### Namespaces

### Ingres

By default Pods are isolated. Ingress provides communication channel to a Service running in a Pod.

#### Ingress Controller

### Deployment

Abstract Layer for managing Pods.

### Replicas

### Sidecars

### DaemonSets

### Roles

### RBAC / Auth

### CNI / Network Plugins

Container Network Interface. Handling cluster overlay networks.

### Helm

Kubernetes Package Manager


## Setup Kubernetes Cluster with kubeadm

### install Docker-CE on Nodes

following the guide:

https://docs.docker.com/install/linux/docker-ce/ubuntu/

- install packages

`> sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common`

- install docker apt keyword

`> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

- verify key

`> sudo apt-key fingerprint <last 8 chars>`

- add docker repository

`> sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- install Docker-CE

`> sudo apt-get update`

`> sudo apt-get install docker-ce docker-ce-cli containerd.io`

- verify installation

`> sudo docker run hello-world`

### Install kubeadm kubectl kubelet

folllowing the tutorial on:

1) https://kubernetes.io/docs/setup/independent/install-kubeadm/

- install packages

`> sudo apt-get update && apt-get install -y apt-transport-https curl`

- add k8s apt keyword

`curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -`

- add kubernetes repository

`> echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list`

- install kubectl kubelet kubeadm

`> sudo apt update`

`> sudo apt install -y kubelet kubeadm kubectl`

`> sudo apt-mark hold kubelet kubeadm kubectl`

2) https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/

- initialize the cluster

`> kubeadm init`

- copy kube-ctl config to home folder

`> mkdir -p $HOME/.kube`

`> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`

`> sudo chown $(id -u):$(id -g) $HOME/.kube/config`

alternatively (if root)

`> export KUBECONFIG=/etc/kubernetes/admin.conf`

### Weave Net Network Plugins

- pass ipv4 traffic to iptables

`> sysctl net.bridge.bridge-nf-call-iptables=1`

`> kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"`

### Add node to the cluster (remeber token from kubeadm init output)

`kubeadm join <ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>`

- check cluster Nodes

`> kubectl get nodes`

### Setup kubectl locally (MacOSX)

`> brew install kubernetes-cli`

`> scp -r user@<ip>:/home/.kube .`

`> cp -R .kube /User/<user> `

### Deploy k8s Dashboard

following tutorial:

https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

`> kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml`

- create role for dashboard for cluster-admin role

`> kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard`

further info of RBAC Auth:

https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole

- show dashboard token for login

`kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep dashboard | awk '{print $1}')`

- Access Dashboard locally using proxy

`> kubectl proxy`


### Setup Helm & Tiller
