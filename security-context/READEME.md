### Setting security context for Pods

[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

#### Create the YAML for an nginx pod that runs with the user ID 101
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
# add security context by manually editing the pod yaml - pod.yaml for details
```