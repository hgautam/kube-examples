### Liveness and readiness probes
[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

Kubernetes supports three mechanisms for implementing liveness and readiness probes: 
1. running a command inside a container
2. making an HTTP request against a container
3. opening a TCP socket against a container.

#### Create an nginx pod with a liveness probe that just runs the command 'ls'
```bash
# Create pod yaml
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml

# create pod
kubectl apply -f pod.yaml

# validate
kubectl describe po nginx | grep -i liveness

# clean up
kubectl delete -f pod.yaml
```
#### Useful command to describe
```bash
kubectl explain pod.spec.containers.livenessProbe # get the exact names
```