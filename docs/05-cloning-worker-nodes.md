### CLONING THE VM

```shell 
shutdown now
```

##### CREATING OTHER NODES from k8s-master (repeat for all VMs):
Next, we will create the additional host or hosts that are required, and make the changes to those hosts needed to complete the build of an Oracle RAC.

STEPS:
1. Shutdown existing VM guest 'shutdown -h now', 
2. from VMware workstation, choose Manage=>clone.
3. current state -> full clone -> name and location (should've created folder)
4. When completed, click close. Do not start up the new host yet. There are some settings that need to be changed
5. Assign new MAC addresses to the NICs in the VM
Any routers that used reserved IPs will use the MAC address to assign them, so we want to make sure this workstation has different MAC addresses than the original

> VM Settings -> Network Adapter -> Advanced -> MAC address -> generate
Do the same for each Network Interface card (NIC) (bridged, NAT, host-only): In this case, I delete other adapters and used only bridged adapter

6. power on VM
7. Login as root
> Network settings -> set IP addresses as done before (Public n Private Network setup)
8. set hostname: run as root or any sudoer
```shell
hostnamectl set-hostname <node1>
```

9. Reboot
```shell
reboot
```




##### CHECK REQUIRED PORTS:

```shell
# ON MASTER NODE:
sudo firewall-cmd --add-port={443,6443,2379-2380,10250,10251,10252,5473,179,4240-4245,6060-6062,9879-9893,9962-9964}/tcp --permanent
sudo firewall-cmd --add-port={4789,8285,8472,51871}/udp --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-all


# ON WORKER NODES:
Enter the following commands on each worker node:

sudo firewall-cmd --add-port={443,10250,30000-32767,5473,179,5473,4240-4245,9879-9893,9962-9964}/tcp --permanent
sudo firewall-cmd --add-port={4789,8285,8472,51871}/udp --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-all

# if you will use another CNI, please check their system requirements
```

Link: 
1. [kubernetes ports-and-protocols](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)
2. [cilium system requirements](https://docs.cilium.io/en/stable/operations/system_requirements/)


Prev: [Configuring master node](04-configuring-master.md)<br>
Next: [Initialize master](06-initialize-master.md)