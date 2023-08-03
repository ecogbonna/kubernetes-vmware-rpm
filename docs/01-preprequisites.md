# Prerequisites

## VM Hardware Requirements

- 16 GB of RAM (Preferably 32 GB)
- 50 GB Disk space

## VMware Workstation Pro

Download and Install [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html) on any of the supported platform:

 - Windows hosts
 - Linux distributions


## Setting up the OS and required packages
Once you have VMware running, download the OEL9 ISO image from
[Oracle](https://edelivery.oracle.com/osdc/faces/Home.jspx). You can also use a different Linux distribution, but
make sure it’s supported by checking the [Kubernetes docs](http://kubernetes.io)

Other steps include:
1. Creating the virtual machine
2. Configuring the network adapter for the VM (Bridged Adapter mode). 
3. Installing the operating system

### Virtual Machine Network

The network used by the Virtual Box virtual machines is `192.168.1.0/24`.


### Pod Network

We will use the cilium default pod CIDR `10.0.0.0/8` to assign IP addresses to pods.


### Service Network

The network used to assign IP addresses to Cluster IP services is `10.64.0.0/20`.


## Running Commands in Parallel with tmux

[tmux](https://github.com/tmux/tmux/wiki) and [mobaxterm](https://mobaxterm.mobatek.net/) can be used to run commands on multiple compute instances at the same time.


## Prepare a sudo user
```shell
# Prepare users:
sudo useradd <user>
sudo passwd <user>

echo "user  ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/user
```


Next: [Compute Resources](02-compute-resources.md)