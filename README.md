# CKA

### Exam Details

The online exam consists of a set of performance-based items (problems) to be solved in a command line and candidates have 3 hours to complete the tasks.

The Certification focuses on the skills required to be a successful Kubernetes Administrator in industry today. This includes these general domains and their weights on the exam:

- Core Concepts - 19%
  - Understand the Kubernetes API primitives.
  - Understand the Kubernetes cluster architecture.
  - Understand Services and other network primitives.

- Installation, Configuration & Validation - 12%
  - Design a Kubernetes cluster.
  - Install Kubernetes masters and nodes.
  - Configure secure cluster communications.
  - Configure a Highly-Available Kubernetes cluster.
  - Know where to get the Kubernetes release binaries.
  - Provision underlying infrastructure to deploy a Kubernetes cluster.
  - Choose a network solution.
  - Choose your Kubernetes infrastructure configuration.
  - Run end-to-end tests on your cluster.
  - Analyse end-to-end tests results.
  - Run Node end-to-end tests.
  - Install and use kubeadm to install, confi gure, and manage Kubernetes clusters

- Security - 12%
  - Know how to configure authentication and authorization.
  - Understand Kubernetes security primitives.
  - Know to configure network policies.
  - Create and manage TLS certificates for cluster components.
  - Work with images securely.
  - Define security contexts.
  - Secure persistent key value store.

- Networking - 11%
  - Understand the networking configuration on the cluster nodes.
  - Understand Pod networking concepts.
  - Understand service networking.
  - Deploy and configure network load balancer.
  - Know how to use Ingress rules.
  - Know how to configure and use the cluster DNS.
  - Understand CNI.

- Cluster Maintenance - 11%
  - Understand Kubernetes cluster upgrade process.
  - Facilitate operating system upgrades.
  - Implement backup and restore methodologies

- Troubleshooting - 10%
  - Troubleshoot application failure.
  - Troubleshoot control plane failure.
  - Troubleshoot worker node failure.
  - Troubleshoot networking.

- Application Lifecycle Management-  8%
  - Understand Deployments and how to per form rolling updates and rollbacks.
  - Know various ways to configure applications.
  - Know how to scale applications.
  - Understand the primitives necessary to create a self-healing application.

- Storage - 7%
  - Understand persistent volumes and know how to create them.
  - Understand access modes for volumes.
  - Understand persistent volume claims primitive.
  - Understand Kubernetes storage objects.
  - Know how to configure applications with persistent storage.

- Scheduling-  5%
  - Use label selectors to schedule Pods.
  - Understand the role of DaemonSets.
  - Understand how resource limits can affect Pod scheduling.
  - Understand how to run multiple schedulers and how to configure Pods to use them.
  - Manually schedule a pod without a scheduler.
  - Display scheduler events.
  - Know how to configure the Kubernetes scheduler.

- Logging / Monitoring-  5%
  - Understand how to monitor all cluster components.
  - Understand how to monitor applications.
  - Manage cluster component logs.
  - Manage application logs.

### Exam Resources

- [Candidate Handbook](https://training.linuxfoundation.org/wp-content/uploads/2020/01/CKA-CKAD-Candidate-Handbook-v1.8.pdf)
- [Curriculum Overview](https://github.com/cncf/curriculum)
- [Exam Tips](https://training.linuxfoundation.org/wp-content/uploads/2020/01/Important-Tips-CKA-CKAD-01.28.2020.pdf)
- [Frequently Asked Questions](https://training.linuxfoundation.org/wp-content/uploads/2020/01/CKA-CKAD-FAQ-01.28.2020.pdf)
- [Certification and Confidentiality Agreement](https://training.linuxfoundation.org/wp-content/uploads/2020/01/Certification-and-Confidentiality-Agreement-CNCF-v1.3-1.pdf)
- [Verify Certification](https://training.linuxfoundation.org/certification/verify/)

---

18/02

# 1 - Core Concepts

## Cluster architecture

### Control Plane Components

![Alt architecture k8s](img/arch_k8s.jpg)

**[kube Api Server](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)** The Kubernetes API server validates and configures data for the api objects which include pods, services, replicationcontrollers, and others. The API Server services REST operations and provides the frontend to the cluster’s shared state through which all other components interact.

The communication hub for all cluster components. It exposes the kubernetes API.

```
kube-apiserver [flags]
```

**[kube Schedule](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/)** The Kubernetes scheduler is a policy-rich, topology-aware, workload-specific function that significantly impacts availability, performance, and capacity. The scheduler needs to take into account individual and collective resource requirements, quality of service requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, deadlines, and so on. Workload-specific requirements will be exposed through the API as necessary.

Assings your app to a worker node, auto-detects which pod to assign to which node based on resource requirements, hardware constraints, etc.

```
kube-scheduler [flags]
```

**[kube controller manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)** The Kubernetes controller manager is a daemon that embeds the core control loops shipped with Kubernetes. In applications of robotics and automation, a control loop is a non-terminating loop that regulates the state of the system. In Kubernetes, a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state. Examples of controllers that ship with Kubernetes today are the replication controller, endpoints controller, namespace controller, and serviceaccounts controller.

Maintains the cluster. handles node failure, replicating components, maintaning the correct amount of pods, etc.

```
kube-controller-manager [flags]
```

**[ETCD](https://etcd.io/docs/v3.4.0/)** Consistent and highly-available key value store used as Kubernetes’ backing store for all cluster data.

If your Kubernetes cluster uses etcd as its backing store, make sure you have a back up plan for those data.

Data store that stores the cluster configuration.

### Node Components

**[Kubelet](https://kubernetes.io/docs/concepts/overview/components/#kubelet)** Runs and manages the containers on the node and talks to the kube API server.

**[kube-proxy](https://kubernetes.io/docs/concepts/overview/components/#kube-proxy)** kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.

kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

kube-proxy uses the operating system packet filtering layer if there is one and it’s available. Otherwise, kube-proxy forwards the traffic itself.

Load balances traffic between application components.

**[Container runtime](https://kubernetes.io/docs/concepts/overview/components/#container-runtime)** The container runtime is the software that is responsible for running containers.

Kubernetes supports several container runtimes: Docker, containerd, CRI-O, and any implementation of the Kubernetes CRI (Container Runtime Interface).

## Understand the Kubernetes API primitives.

## Understand the Kubernetes cluster architecture.

## Understand Services and other network primitives.

---
# References

- [What is kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)
- [12factor app](https://12factor.net/pt_br/)
- [Pod of minerva](https://interactive.linuxacademy.com/diagrams/ThePodofMinerva.html)
- [CKA](https://www.cncf.io/certification/cka/)
- [Kube-api](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
- [Kube schedule](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/)
- [Kube controller manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)
- [Etcd](https://kubernetes.io/docs/concepts/overview/components/#etcd)
- [Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)
- [Kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)
- [Container runtime](https://kubernetes.io/docs/concepts/overview/components/#container-runtime)
- [Kubernetes components](https://kubernetes.io/docs/concepts/overview/components/)
- [Kubernetes field selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/field-selectors/)
- [Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Proxy](https://kubernetes.io/docs/concepts/services-networking/service/#virtual-ips-and-service-proxies)
- [High availability](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/)
- [HA topology](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/)
- [Configure upgrade etcd](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/)
