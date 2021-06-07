### Assign CPU Resources to Containers and Pods

[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/)

* metrics-server needs to enabled to enforce requests/limits
* request: requirement
* limit: max capacity

#### Create a pod with resource limits
```bash
# enable metrics server
minikube addons enable metrics-server

# creare namespace
kubectl create ns cpu-example

# apply yaml
kubectl apply -f cpu-request-limit.yaml --namespace=cpu-example
```
#### Create an nginx pod with requests cpu=100m,memory=256Mi and limits cpu=200m,memory=512Mi
```bash
kubectl run nginx --image=nginx --requests='cpu=100m,memory=256Mi' --limits='cpu=200m,memory=512Mi' --dry-run=client --restart=Never -o yaml > limit-pod.yaml

# apply yaml
kubectl apply -f limit-pod.yaml

# clean up
kubectl delete  pod nginx
```