### Smoke Test


```shell
kubectl get nodes
NAME     STATUS   ROLES           AGE   VERSION
master   Ready    control-plane   29m   v1.26.1
node1    Ready    <none>          25m   v1.26.1
node2    Ready    <none>          25m   v1.26.1


kubectl get nodes --show-labels
kubectl label node node1 node-role.kubernetes.io/node1=worker

# should you want to unlabel
kubectl label node node1  node-role.kubernetes.io/k8s-node1-							
node/node1 unlabeled


kubectl label node node1 node-role.kubernetes.io/node1=worker
node/node1 labeled

kubectl label node node2 node-role.kubernetes.io/node2=worker
node/node2 labeled

kubectl get nodes
NAME     STATUS   ROLES           AGE   VERSION
master   Ready    control-plane   35m   v1.26.1
node1    Ready    node1           31m   v1.26.1
node2    Ready    node2           31m   v1.26.1


# You verify all the cluster component health statuses using the following command:

kubectl get --raw='/readyz?verbose'
kubectl get componentstatus


# sometimes, we might need to run "systemctl restart kubelet" to get the nodes to "Ready" status. with Calico, I experienced it. with Cilium, I never did.



# Deploy A Sample Nginx Application:

cat <<EOF | kubectl apply -f -
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80 
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector: 
    app: nginx
  type: NodePort  
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32000		
EOF


curl -s http://192.168.1.71:32000/



 k get svc
NAME            TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.64.0.1     <none>        443/TCP        105m
nginx-service   NodePort    10.64.8.171   <none>        80:32000/TCP   29s

 k get deploy -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
nginx-deployment   2/2     2            2           43s   nginx        nginx:latest   app=nginx

k get pods -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP            NODE    NOMINATED NODE   READINESS GATES
nginx-deployment-6b7f675859-rcrq9   1/1     Running   0          54s   10.244.2.15   node2   <none>           <none>
nginx-deployment-6b7f675859-v6ccf   1/1     Running   0          54s   10.244.1.6    node1   <none>           <none>


```


Prev: [Install and Validate Cilium](09-install-validate-cilium.md)<br>
Next: [Prepare admin environment](11-kubectl-env.md)