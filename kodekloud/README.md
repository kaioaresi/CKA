# CKA (Certified Kubernetes Administrator)
> https://www.cncf.io/certification/cka/

- Core Concepts - `19%`
  - Cluster architecture
  - Services & other network primitives
  - API primitives
- Scheduling - `5%`
  - Labels & Selectors
  - Daemon Sets
  - Configure kubernetes scheduler
  - Resource limits
  - Multiple scheduler
  - Manual scheduling
  - Scheduler events
- Logging / Monitoring - `5%`
  - Monitor cluster components
  - Monitor cluster component logs
  - Monitor Applications
  - Application logs
- Application Lifecycle Management - `8%`
  - Rolling updates and rollbacks in deploy
  - Configure Applications
  - Scale applications
  - Self-healing application
- Cluster Maintenance - `11%`
  - Cluster upgrade process
  - Operationg system upgrades
  - Backup and restore methodologies
- Security - `12%`
  - Authentication & Authentization
  - Kubernetes Security
  - Network policies
  - TLS certificates for cluster components
  - Images securely
  - Security contexts
  - Secure persistent key value store
- Storage - `7%`
  - persistent volumes
  - Access modes for volumes
  - Persistent volume claims
  - Kubernetes storage object
  - Configure application with persistent store
- Networking - `11%`
  - Pre-requisites
  - Network, switching, Routing, Tools
  - Pre-requisites - DNS and CoreDNS
  - Networking Configuration on cluster nodes
  - POD networking Concepts
  - Network loadbalancer
  - Ingress
  - Cluster DNS
  - CNI
  - Pre-requisites - network namespaces
  - Pre-requisites - Network in docker
- Installation, Configuration & Validation - `12%`
- Troubleshooting - `10%`
  - Application failure
  - Control plane failure
  - Worker node failure
  - Networking

# Informações sobre a certificação

## Use the code - DEVOPS15 - while registering for the CKA or CKAD exams at Linux Foundation to get a 15% discount.

- [Certified Kubernetes Administrator:](https://www.cncf.io/certification/cka/)
- [Exma tips](https://training.linuxfoundation.org/wp-content/uploads/2020/03/Important-Tips-CKA-CKAD-March-2020.pdf)
- [Exma tips2](https://training.linuxfoundation.org/wp-content/uploads/2020/04/CKA-CKAD-Candidate-Handbook-v1.10.pdf)
- [Github CNCF curriculum](https://github.com/cncf/curriculum)
- [CKA Curriculum V1.17](https://github.com/cncf/curriculum/blob/master/CKA_Curriculum_V1.17.pdf)

## Kubernetes the hard way

- https://github.com/mmumshad/kubernetes-the-hard-way

---

# Core Concepts - `19%`

## Cluster architecture

**Master node - Control plane**

- Manage
- Plan
- Schedule
- Monitor
- Nodes

### Master Components

- ETCD - armazena dados do cluster, formato key-value
- Scheduler - responsável por verificar se existe recurso em um node e realizar o deploy do pod
-  Controller-manager - responsável por gerenciar controllers e nodes
- Kube-api - responsável por todas operações dentro do cluster


**Worker node**

- Host aplication as containers

### Nodes components - all nodes

- Container runtimer (docker, ContainerD e etc)
- Kubelet - recebe e executa instruções do contral plane
- kube-proxy - responsável pela comunicação entre componentes do cluster, como services, pods e etc.

###**Conrol plane components**

#### ETCD

**What is [ETCD](https://kubernetes.io/docs/concepts/overview/components/#etcd)?**
> https://etcd.io/docs/v3.4.0/

ETCD is a distributed reliable key-value store that is simple, secure & fast, your default port is `2379`.

**Explore ETCD**

```
kubectl exec etcd-master -n kube-system etcdctl get / --prefix -key-only
```

#### Kube-API

É o unico componente que comunica com o ETCD, todos demais componentes devem se comunicar com o kube-api para que o kube-api se comunique com o ETCD.


**Create pod flow**

1 - Authenticate user
2 - Validate request
3 - Retrieve data
4 - Update ETCD
5 - Scheduler
6 - Kubelet
7 - Update ETCD com o status do deploy do pod.

**Path configfile - setup kubeadm**

- `/etc/kubernetes/manifests/kube-apiserver.yaml`

- `/etc/systemd/system/kube-apiserver.service`

#### Controller-manager

- Watch status
- Remediate situation

Manager and monitoring every single controller:

- Deployment
- Replicaset
- ReplicationController
- Statefulset
- CronJob
- Namespace-controller
- Endpoing-controller
- Job-controller
- PV- protection-controller
- Pv-binder-controller

**Node controller**

Check node every `5s` and is the node doesn't retry in `40s`, it's mark as `unreachable` and `5m` later `notReady`


#### Kube-scheduler

Responsable to say where each pod go to each node, and to the Kubelet plays. kube-scheduler try to find the best node to the pod, first it make the filter nodes and after rank Nodes.

#### Kubelet

It's node's agente, it receive control planes commands, and play, it regiser node, create pods, monitor node & pods.

#### Kube-proxy

![Alt kube-proxy](../img/kube-proxy.jpg)

#### Pods

It's the must basic kubernetes object, it can have one or more container inside then.

> Tip: Inside the pod, it's good practices don't have the same kind of container inside then

**Create new pod aplication**
> https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/

```
kubectl run --image=nginx nginx-app --port=80
```

YAML file

```
apiVersion: v1
kind: Pod
metadata:
  name: exmplo-pod
  labels:
    app: exemplo
spec:
  containers:
    - name: container-exemplo
      image: nginx
```

#### Controllers

**Replicaset vs Replication Controller**
> https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/


It's controller to keep the pods up, if happening some error it try to up the pods again

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc-myapp
  labels:
    app: myappp
    type: front-end
spec:
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
```

```
apiVersion: app/v1
kind: ReplicaSet
metadata:
  name: rs-myapp
  labels:
    app: myapp
    type: front-end
spec:
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  selector: # this options doesn't existe on ReplicationController
    matchLabels:
      type: myapp
```

---
## Certification Tip!

Here's a tip!

As you might have seen already, it is a bit difficult to create and edit YAML files. Especially in the CLI. During the exam, you might find it difficult to copy and paste YAML files from browser to terminal. Using the kubectl run command can help in generating a YAML template. And sometimes, you can even get away with just the kubectl run command without having to create a YAML file at all. For example, if you were asked to create a pod or deployment with specific name and image you can simply run the kubectl run command.

Use the below set of commands and try the previous practice tests again, but this time try to use the below commands instead of YAML files. Try to use these as much as you can going forward in all exercises

Reference (Bookmark this page for exam. It will be very handy):

https://kubernetes.io/docs/reference/kubectl/conventions/

Create an NGINX Pod

```
kubectl run --generator=run-pod/v1 nginx --image=nginx
```


Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

```
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml
```


Create a deployment

```
kubectl create deployment --image=nginx nginx
```

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

```
kubectl create deployment --image=nginx nginx --dry-run -o yaml
```

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)

```
kubectl create deployment --image=nginx nginx --dry-run -o yaml > nginx-deployment.yaml
```

Save it to a file, make necessary changes to the file (for example, adding more replicas) and then create the deployment.

