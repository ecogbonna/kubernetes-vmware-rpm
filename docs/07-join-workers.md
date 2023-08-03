### Configuring worker nodes with kubeadm

```shell
kubeadm join master:6443 --token 68f0ys.jwfefd7r03lqtupm \
        --discovery-token-ca-cert-hash sha256:cad6144cd55c8f0f91e1ff205e3ba2a465ea5b9bd866c43f58230f4546eab281
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

# kubectl get nodes
NAME     STATUS     ROLES           AGE     VERSION
master   NotReady   control-plane   30m     v1.26.1
node1    NotReady   <none>          2m57s   v1.26.1
node2    NotReady   <none>          67s     v1.26.1

# kubectl describe node k8s-node1
Conditions:
...
KubeletNotReady              container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:Network plugin r

```


Prev: [Initialize master](06-initialize-master.md)<br>
Next: [Install Helm](08-install-helm.md)