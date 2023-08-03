### Running `kubeadm Init` to initialize the master
```shell
# check the --pod-network-cidr requirements for your CNI of choice e.g. Flannel uses --pod-network-cidr=10.244.0.0/16

# sudo kubeadm init \
  --service-cidr=10.64.0.0/20\
  --control-plane-endpoint=master --dry-run >> init.log
  
# # sudo kubeadm init \
  --service-cidr=10.64.0.0/20 \
  --control-plane-endpoint=master \
  --apiserver-advertise-address=192.168.1.70

...
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of control-plane nodes by copying certificate authorities
and service account keys on each node and then running the following as root:

  kubeadm join master:6443 --token 68f0ys.jwfefd7r03lqtupm \
        --discovery-token-ca-cert-hash sha256:cad6144cd55c8f0f91e1ff205e3ba2a465ea5b9bd866c43f58230f4546eab281 \
        --control-plane

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join master:6443 --token 68f0ys.jwfefd7r03lqtupm \
        --discovery-token-ca-cert-hash sha256:cad6144cd55c8f0f91e1ff205e3ba2a465ea5b9bd866c43f58230f4546eab281
		
		
# --control-plane-endpoint=master makes it possible to join any number of control-plane nodes i.e. for HA clusters, without it, we can only join worker nodes

```


Prev: [Clone worker nodes](05-cloning-worker-nodes.md)<br>
Next: [Join worker nodes](07-join-workers.md)