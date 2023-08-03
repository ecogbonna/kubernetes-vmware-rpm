### Setup environments where you will be running kubectl commands
```shell
$ sudo dnf install -y bash-completion

# CREATING AN ALIAS

~/.bash_profile:

echo "alias k=kubectl" >> ~/.bash_profile
echo "source <(kubectl completion bash)" >> ~/.bash_profile
echo "source <(kubectl completion bash | sed s/kubectl/k/g)" >> ~/.bash_profile
. .bash_profile

$ k get nodes
NAME         STATUS   ROLES           AGE     VERSION
k8s-master   Ready    control-plane   7h19m   v1.27.3
k8s-node1    Ready    <none>          7h10m   v1.27.3
k8s-node2    Ready    <none>          7h10m   v1.27.3

```


Prev: [Smoke test](10-smoke-test.md)<br>
Next: [Observability](12-observability.md)