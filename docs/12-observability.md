
## Deploying Metrics-server

Step 1:

```bash
# Adding "serverTLSBootstrap: true" in cluster’s kubelet ConfigMap kubelet-config.

kubectl get cm -A
kubectl edit cm kubelet-config -n kube-system
kubectl get cm kubelet-config -n kube-system -o yaml | grep server
    serverTLSBootstrap: true
	

Adding "serverTLSBootstrap: true" into config.yaml under /var/lib/kubelet/ on all cluster nodes.
vi /var/lib/kubelet/config.yaml
```

To make the newly added configuration to work, we need to restart kubelet daemon on all nodes:
```bash
sudo systemctl restart kubelet.service
```

check if there’s any CSR created:
```bash
kubectl get csr
kubectl certificate approve <CSR>
``` 


Step 2:
```bash
curl -LO https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/high-availability.yaml
sudo sed -i 's/policy\/v1beta1/policy\/v1/g' high-availability.yaml

/usr/bin/kubectl apply -f high-availability.yaml
```




## Validate your Installation using Cilium CLI

```bash
CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/master/stable.txt)
CLI_ARCH=amd64
if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}


# cilium status --wait
    /¯¯\
 /¯¯\__/¯¯\    Cilium:             OK
 \__/¯¯\__/    Operator:           OK
 /¯¯\__/¯¯\    Envoy DaemonSet:    disabled (using embedded mode)
 \__/¯¯\__/    Hubble Relay:       disabled
    \__/       ClusterMesh:        disabled

Deployment             cilium-operator    Desired: 2, Ready: 2/2, Available: 2/2
DaemonSet              cilium             Desired: 3, Ready: 3/3, Available: 3/3
Containers:            cilium             Running: 3
                       cilium-operator    Running: 2
Cluster Pods:          6/6 managed by Cilium
Helm chart version:    1.13.4
Image versions         cilium             quay.io/cilium/cilium:v1.13.4@sha256:bde8800d61aaad8b8451b10e247ac7bdeb7af187bb698f83d40ad75a38c1ee6b: 3
                       cilium-operator    quay.io/cilium/operator-generic:v1.13.4@sha256:09ab77d324ef4d31f7d341f97ec5a2a4860910076046d57a2d61494d426c6301: 2

cilium connectivity test
```

Prev: [Prepare admin environment](11-kubectl-env.md)<br>
Next: [Cleanup and next steps](13-cleanup-next-steps.md)