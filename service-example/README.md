### Kubernetes Service example

Accessing an application via Service object
[K8s documentation link](https://kubernetes.io/docs/tasks/access-application-cluster/service-access-application-cluster/)

```bash
# apply yaml
kubectl apply -f hello-world.yaml

# Display information about the Deployment
kubectl get deployments hello-world
kubectl describe deployments hello-world

# Display information about your ReplicaSet objects
kubectl get replicasets
kubectl describe replicasets

# Create a Service object that exposes the deployment
kubectl expose deployment hello-world --type=NodePort --name example-service

# Display information about the Service
kubectl describe service example-service
```