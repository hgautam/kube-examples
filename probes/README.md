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
#### Create an nginx pod (that includes port 80) with an HTTP readinessProbe on path '/' on port 80. Again, run it, check the readinessProbe
```bash
# create yaml
kubectl run nginx --image=nginx --restart=Never --port=80 --dry-run=client -o yaml > pod2.yaml

# apply yaml
kubectl apply -f pod2.yaml

# clean up
kubectl delete -f pod2.yaml
```