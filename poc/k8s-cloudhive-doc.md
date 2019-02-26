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

Abstract Layer for managing Pods

### CNI / Network Plugins

Container Network Interface. Handling cluster overlay network.

### Replicas

### Helm & Tiller

### Sidecars

### DaemonSets

### Roles


## Setup Kubernetes Cluster with kubeadm

### install Docker-CE on Nodes

following the guide:

https://docs.docker.com/install/linux/docker-ce/ubuntu/

### Install kubeadm kubectl kubelet

folllowing the tutorial on:

1) https://kubernetes.io/docs/setup/independent/install-kubeadm/

2) https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/

### Waveshare

`> sysctl net.bridge.bridge-nf-call-iptables=1`

`> kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"`

- create rbac file for weave-net
```
yaml
```


### Add node to the cluster

`kubeadm join <ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>`

- check cluster Nodes

`> kubectl get nodes`

### Deploy Dashboard

`> kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml`

### Setup kubectl locally (MacOSX)

`> brew install kubernetes-cli`

`> scp -r user@<ip>:/home/.kube .`

`> cp -R .kube /User/<user> `

### create admin.yml for dashboard admin user

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
```

`> kubectl apply -f <admin>.yml`

### create / show user token for login

`kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')`

### Access Dashboard locally using proxy

`> kubectl proxy`


### Setup Helm
