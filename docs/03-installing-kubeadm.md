# Installing Kubernetes on OEL9 using kubeadm
See the required prerequisites at [Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)


Brief overview of steps:
1. Installing kubeadm: Configuring the master with kubeadm
	- CONFIGURE LINUX COMPONENTS (MAC Address, Network adapters, Ports, SWAP)
	- Installing a container runtime
		- Install and configure prerequisites 
			- Forwarding IPv4 and letting iptables see bridged traffic (sysctl)
		- Container runtime: installing containerd from apt-get or dnf
		- cgroup drivers
			- systemd cgroup driver (recommended)
				- Configuring the systemd cgroup driver: containerd
	- Installing kubeadm, kubelet and kubectl
2. Cloning the Master node to create worker nodes: remember to change MAC Address IP, and hostname
3. Initializing your control-plane node.
	- plan for high availability by specifying the --control-plane-endpoint to set the shared endpoint for all control-plane nodes. (Recommended) 
	- Choose a Pod network add-on, and verify whether it requires any arguments to be passed to kubeadm init: flannel requires --pod-network-cidr (Recommended) 
4. Save the node join command with the token.
5. Join the worker node to the master node (control plane) using kubeadm join command.
6. Install and Validate Cilium CNI for flat-inter-pod networking.
7. Validate all cluster components and nodes.
8. Install Kubernetes Metrics Server
9. Using the cluster from your local machine
	- (Optional) Controlling your cluster from machines other than the control-plane node
10. (Optional) Proxying API Server to localhost
11. Cleanup 


Prev: [Compute Resources](02-compute-resources.md)<br>
Next: [Configuring master node](04-configuring-master.md)