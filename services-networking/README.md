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
# https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/
kubectl create deploy nginx --image=nginx --replicas=2
# expose the deployment
kubectl expose deployment nginx --port=80
# create network policy
kubectl create -f policy.yaml

# make sure that your cluster's network provider supports Network Policy (https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/#before-you-begin)
kubectl run busybox --image=busybox --rm -it --restart=Never -- wget -O- http://nginx:80 --timeout 2 # This should not work. --timeout is optional here. But it helps to get answer more quickly (in seconds vs minutes)
kubectl run busybox --image=busybox --rm -it --restart=Never --labels=access=granted -- wget -O- http://nginx:80 --timeout 2  # This should be fine
```
#### https://github.com/bmuschko/ckad-prep/blob/master/6-services-and-networking.md#routing-traffic-to-pods-from-inside-and-outside-of-a-cluster
```bash
# Create a deployment named myapp that creates 2 replicas for Pods with the image nginx. Expose the container port 80.
kubectl create deploy myapp --image=nginx --replicas=2 --port=80 --dry-run=client -o yaml
# Expose the Pods so that requests can be made against the service from inside of the cluster
kubectl expose deploy myapp --port=80 --target-port=80 --dry-run=client -o yaml
# Create a temporary Pods using the image busybox and run a wget command against the IP of the service.
kubectl run busybox --image=busybox -it --restart=Never -- /bin/sh
wget myapp
# Change the service type so that the Pods can be reached from outside of the cluster.
kubectl edit svc myapp # change type to NodePort
# access it from localhost. In case of minikube,
kubectl cluster-info
curl http://192.168.64.132:30319
# clean up
kubectl delete deploy myapp;kubectl delete svc myapp
# create a nginx pod
kubectl run nginx --image=nginx --restart=Never 
# expose the pod on port 80
kubectl expose pod nginx --port=80 --target-port=80 --dry-run=client -o yaml
# create a busybox pod to access the above po
kubectl run busybox --image=busybox --restart=Never -it --rm -- /bin/sh -c 'wget -O- http://10.110.179.42'
# clean up
kubectl delete po nginx; kubectl delete svc nginx
```
#### https://github.com/bmuschko/ckad-prep/blob/master/6-services-and-networking.md#restricting-access-to-and-from-a-pod
```bash
```