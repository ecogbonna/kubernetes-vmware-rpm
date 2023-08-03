# Installing Kubernetes on RPM distributions using kubeadm and VMware

## Cluster Details

* [Kubernetes](https://github.com/kubernetes/kubernetes) 1.27.0
* [Container Runtime](https://github.com/containerd/containerd) 1.5.9
* [CNI Container Networking](https://github.com/containernetworking/cni) 0.8.6
* [Cilium Networking](https://docs.cilium.io/en/stable/installation/k8s-install-kubeadm/)
* [etcd](https://github.com/coreos/etcd) v3.5.3
* [CoreDNS](https://github.com/coredns/coredns) v1.9.4

### Node configuration

We will be building the following:

* One control plane node (`master`) running the control plane components as static pods.
* Two worker nodes (`node1` and `node2`)


## Labs

* [Prerequisites](docs/01-prerequisites.md)
* [Provisioning Compute Resources](docs/02-compute-resources.md)
* [Installing overview](docs/03-installing-kubeadm.md)
* [Configuring the master with kubeadm](docs/04-configuring-master.md)
* [Cloning worker nodes](docs/05-cloning-worker-nodes.md)
* [Initializing Master node](docs/06-initialize-master.md)
* [Joining worker nodes](docs/07--join-workers.md)
* [Installing helm](docs/08-install-helm.md)
* [Installing and Validating Cilium](docs/09-install-validate-cilium.md)
* [Smoke Test](docs/10-smoke-test.md)
* [Preparing kubectl environment](docs/11-kubectl-env.md)
* [Observability](docs/12-observability.md)
* [Cleanup and next steps](docs/13-cleanup-next-steps.md)
