### Clean up or Restart installation:

```shell
# MASTER NODE
export KUBECONFIG=/etc/kubernetes/admin.conf
kubectl drain master --delete-emptydir-data --force --ignore-daemonsets

kubeadm reset && rm -rf /etc/cni/net.d
iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X
ipvsadm -C


# WORKER NODES
kubeadm reset && rm -rf /etc/cni/net.d
iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X
ipvsadm -C
```

### Further setup:

Components
Depending on the expectations and requirements, below components may be used:

- External Load Balancer for K8s Services (type: LoadBalancer), for example: MetalLB, Porter
- Ingress Controller (reverse proxy, HTTP router), for example: Nginx, Contour, HAProxy, Traefik
- Cert Manager, for example: Let’s Encrypt, BuyPass
- External DNS, for example: ExternalDNS
- Persistent Volume, for example: NFS, GlusterFS


Prev: [Observability](12-observability.md)