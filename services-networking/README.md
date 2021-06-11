### Services and Networking

* [Kubernetes documentation reference](https://kubernetes.io/docs/concepts/services-networking/)
* [Network policies reference](https://kubernetes.io/docs/concepts/services-networking/network-policies/)


* Services work with deployment objects and Pods.
* The command to create a service object:
```bash
kubectl expose
```
* [The reference to command documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#expose)

```bash
# Create a pod with image nginx called nginx and expose its port 80
kubectl run nginx --image=nginx --restart=Never --port=80 --expose --dry-run=client -o yaml

# Confirm that ClusterIP has been created. Also check endpoints
kubectl get svc nginx
kubectl get ep

# Get service's ClusterIP, create a temp busybox pod and 'hit' that IP with wget
kubectl run busybox --image=busybox --restart=Never -it --rm -- /bin/sh -c 'wget -O- http://172.17.0.3'
kubectl logs busybox

# Convert the ClusterIP to NodePort for the same service and find the NodePort port
kubectl edit svc nginx
# change type from clusterip to nodeport
kubectl get svc

# Create a deployment called foo using image 'dgkanatsios/simpleapp' (a simple server that returns hostname) and 3 replicas. Label it as 'app=foo'.
kubectl create deploy foo --image=dgkanatsios/simpleapp --replicas=3 --port=8080 --dry-run=client -o yaml 

# Create a service that exposes the deployment on port 6262. Verify its existence, check the endpoints
kubectl expose deploy foo --port=6262 --target-port=8080
kubectl get service foo # you will see ClusterIP as well as port 6262
kubectl get endpoints foo # you will see the IPs of the three replica nodes, listening on port 8080

# Create a temp busybox pod and connect via wget to foo service. Verify that each time there's a different hostname returned. 
kubectl run busybox --image=busybox --restart=Never -it -- /bin/sh
wget -O- foo:6262

# Create an nginx deployment of 2 replicas, expose it via a ClusterIP service on port 80. Create a NetworkPolicy so that only pods with labels 'access: granted' can access the deployment and apply it
kubectl create deploy nginx --image=nginx --replicas=2
# create network policy
kubectl create -f policy.yaml

# make sure that your cluster's network provider supports Network Policy (https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/#before-you-begin)
kubectl run busybox --image=busybox --rm -it --restart=Never -- wget -O- http://nginx:80 --timeout 2                          # This should not work. --timeout is optional here. But it helps to get answer more quickly (in seconds vs minutes)
kubectl run busybox --image=busybox --rm -it --restart=Never --labels=access=granted -- wget -O- http://nginx:80 --timeout 2  # This should be fine
```
#### https://github.com/bmuschko/ckad-prep/blob/master/6-services-and-networking.md#routing-traffic-to-pods-from-inside-and-outside-of-a-cluster