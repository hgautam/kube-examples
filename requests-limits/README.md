### Assign CPU Resources to Containers and Pods

[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/)

Create a pod with resource limits
```bash
# enable metrics server
minikube addons enable metrics-server

# creare namespace
kubectl create ns cpu-example

# apply yaml
kubectl apply -f cpu-request-limit.yaml --namespace=cpu-example
```