### Assign CPU Resources to Containers and Pods

* The resource constraints can be specified at the namespace level via a quota object or at a pod level.
* Below examples set constraints at the Pod level

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
kubectl apply -f cpu-request-limit.yaml -n cpu-example
```
#### Create an nginx pod with requests cpu=100m,memory=256Mi and limits cpu=200m,memory=512Mi
```bash
kubectl run nginx --image=nginx --requests='cpu=100m,memory=256Mi' --limits='cpu=200m,memory=512Mi' --dry-run=client --restart=Never -o yaml > limit-pod.yaml

# apply yaml
kubectl apply -f limit-pod.yaml

# to validate
kubectl top pod cpu-demo --namespace=cpu-example

# clean up
kubectl delete  pod nginx
```