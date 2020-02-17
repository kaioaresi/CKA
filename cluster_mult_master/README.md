# Goal

![Cluster mult master](https://d33wubrfki0l68.cloudfront.net/d1411cded83856552f37911eb4522d9887ca4e83/b94b2/images/kubeadm/kubeadm-ha-topology-stacked-etcd.svg)

# Create load balancer for kube-apiserver

1. Create a kube-apiserver load balancer with a name that resolves to DNS.

  * In a cloud environment you should place your control plane nodes behind a TCP forwarding load balancer. This load balancer distributes traffic to all healthy control plane nodes in its target list. The health check for an apiserver is a TCP check on the port the kube-apiserver listens on (default value :6443).

  * It is not recommended to use an IP address directly in a cloud environment.

  * The load balancer must be able to communicate with all control plane nodes on the apiserver port. It must also allow incoming traffic on its listening port.

  * HAProxy can be used as a load balancer.

  * Make sure the address of the load balancer always matches the address of kubeadm’s ControlPlaneEndpoint.



## HAproxy

```
apt-get update && apt-get install -y haproxy
```

```
cd /etc/haproxy
vim haproxy.cfg
```

```
frontend kubernetes
  mode tcp
  bind <ip da instancia>:6443
  option tcplog
  default_backend k8s-master

backend k8s-masters
  mode tcp
  balance roundrobin
  option tcp-check
  server <name master 1> <ip interno master 1>:6443 check fall 3 rise 2
  server <name master 2> <ip interno master 2>:6443 check fall 3 rise 2
  server <name master 3> <ip interno master 3>:6443 check fall 3 rise 2
```

```
systemctl restart haproxy
```
## Test

```
nc -v LOAD_BALANCER_IP PORT
```

## Install docker



```
curl -fsSl https://get.docker.com | bash
```

__Change cgroupfs to systemd__

```
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

mkdir -p /etc/systemd/system/docker.service.d
systemctl daemon-reload
systemctl restart docker
```


```
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -fsSl https://get.docker.com | bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update && sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

```
vim /etc/hosts

<ip do haproxy> <hostname haproxy>
```

## Master 1

```
sudo kubeadm init --control-plane-endpoint "LOAD_BALANCER_DNS:LOAD_BALANCER_PORT" --upload-certs
```

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

**Network**
```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

## Join others masters

Get the stdout with word `control-plane` on master-1, exec then on the others master nodes

```
kubeadm join haproxy-master-1:6443 --token 0r7h3h.evqy8tbhcm5mmwt6 \
  --discovery-token-ca-cert-hash sha256:ce5a96e0186c12eaef811b4f803ded69a9fcaf4114ab91ca86a439b1996793a5 \
  --control-plane --certificate-key 53fd8e4c83ef27212a9646b1eb5ce473ce710ab2641cab29eed7708bac26a5f0
```

---

# Referencias

- [Creating Highly Available clusters with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/)
- [COMO CRIAR UM CLUSTER KUBERNETES MULTI MASTER](https://www.youtube.com/watch?v=-W8b4d8lTNY)
- [HAproxy](http://www.haproxy.org/#docs)
- [Container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
