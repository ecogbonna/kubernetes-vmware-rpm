### Installing kubeadm: Configuring the master with kubeadm
#### Configure Linux Components (SELinux, SWAP and sysctl)

##### Verify the MAC address and product_uuid are unique for every node

> we will ensure this during VM Cloning


##### CHECK NETWORK ADAPTERS

> we're using only bridge network adapter....much simpler to manage



##### DISABLING SWAP
The Kubelet won’t run if swap is enabled, so you’ll disable it with the following command:

```shell
# disable swap
sudo swapoff -a && sudo sed -i '/ swap / s/^/#/' /etc/fstab
```



#### Installing a container runtime
##### Install and configure prerequisites

```shell
# Forwarding IPv4 and letting iptables see bridged traffic
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF	
 
sudo modprobe overlay
sudo modprobe br_netfilter	  
	 
# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system	 
	

# Verify that the br_netfilter, overlay modules are loaded by running the following commands:

lsmod | grep br_netfilter
lsmod | grep overlay	 

# Verify that the net.bridge.bridge-nf-call-iptables, net.bridge.bridge-nf-call-ip6tables, and net.ipv4.ip_forward system variables are set to 1 in your sysctl config by running the following command:

sysctl net.bridge.bridge-nf-call-iptables net.bridge.bridge-nf-call-ip6tables net.ipv4.ip_forward

```

##### Container Runtime: Installing Containerd from apt-get or dnf


---
**NOTE**

*1. You need a container engine, high-level container runtime, and a low-level container runtime.*

*2. On OEL9, no need to install docker. RHEL distributions will have podman installed already.*

*3. I chose containerd (i.e. podman -> containerd -> runc), you can go with CRI-O, Docker Engine (cri-dockerd adapter), or Mirantis Container Runtime.*

---

```shell
# which podman
/usr/bin/podman
[root@master ~]# podman version
Client:       Podman Engine
Version:      4.4.1
API Version:  4.4.1
Go Version:   go1.19.6
Built:        Fri May 12 09:55:18 2023
OS/Arch:      linux/amd64
```





##### Install required packages
```shell 
# Install required packages
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# Add Docker repo
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo


# docker was deprecated in RHEL distributions, so stick to podman
sudo yum update -y && yum install -y containerd.io


# refresh the config, this will help you avoid some issues I encountered earlier
sudo mkdir -p /etc/containerd 
sudo containerd config default > /etc/containerd/config.toml

sudo systemctl restart containerd && sudo systemctl enable containerd

```

Links:
1. [containerd github](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)
2. [containerd docker](https://docs.docker.com/engine/install/centos/)
3. [containerd computingforgeeks](https://computingforgeeks.com/)
4. [kubernetes containerd](install-kubernetes-cluster-on-centos-with-kubeadm/?amp)




##### Configuring the systemd cgroup driver
---
**NOTE**

To use the systemd cgroup driver in /etc/containerd/config.toml with runc, set:

[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true

```shell
vi /etc/containerd/config.toml
sudo systemctl restart containerd
```
---




##### Overriding The Sandbox (Pause) Image
In your containerd config you can overwrite the sandbox image by setting the following config:

[plugins."io.containerd.grpc.v1.cri"]
  sandbox_image = "registry.k8s.io/pause:3.9"
  
  
sudo systemctl restart containerd
 

> Do this to avoid WARNINGs during kubeadm init. kubeadm uses 3.9, but containered used 3.6





#### INSTALLING kubeadm, kubelet and kubectl
##### ADDING THE KUBERNETES YUM REPO

```shell 
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

	# Make sure no whitespace exists after EOF if you’re copying and pasting
	
	
	
# Set SELinux in permissive mode (effectively disabling it)
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum erase -y kubelet kubeadm kubectl --disableexcludes=kubernetes
sudo dnf install -y kubelet-1.26.1 kubeadm-1.26.1 kubectl-1.26.1 --disableexcludes=kubernetes

systemctl enable --now kubelet && systemctl start kubelet

```



##### CONFIGURING NAME RESOLUTION FOR ALL THREE HOSTS

```shell 
cat /etc/hosts

# Kubernetes Bridged Adapter 
192.168.1.70   master  master.localdomain
192.168.1.71   node1  node1.localdomain
192.168.1.72   node2  node2.localdomain
```

Prev: [Installing kubeadm](03-installing-kubeadm.md)<br>
Next: [Clone worker nodes](05-cloning-worker-nodes.md)