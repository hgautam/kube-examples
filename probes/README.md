### Liveness and readiness probes
[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

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