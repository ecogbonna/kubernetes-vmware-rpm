### Install Helm
```shell
wget https://get.helm.sh/helm-v3.12.2-linux-amd64.tar.gz


tar -xvf helm-v3.12.2-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin


rm helm-v3.12.2-linux-amd64.tar.gz
rm -rf linux-amd64


helm version
```

Link: 
1. [Helm github](https://github.com/helm/helm/releases)





Prev: [Join worker nodes](07-join-workers.md)<br>
Next: [Install and Validate Cilium](09-install-validate-cilium.md)