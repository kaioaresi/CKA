# CKA
---

# O que é `kubernetes` ?

Kubernetes (K8s) é um produto Open Source utilizado para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêiner.

![Alt arc k8s](./img/arch_k8s.jpg)

## Field selector

```
kubectl get pods --field-selector status.phase=Running
```

## Exemplo

```
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF
```


---
# References

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
