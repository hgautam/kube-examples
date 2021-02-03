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

# List the pods that are running the Hello World application
kubectl get pods --selector="run=node-port-example" --output=wide

# Get ip address of your nodes
# since our cluster is running on minikube
kubectl cluster-info
minikube ip

# List nodeport
kubectl describe service example-service | grep NodePort

# Access the application
curl http://<node-ip:node-port>
curl  http://192.168.64.88:30276
Hello Kubernetes!

# Delete the Service
kubectl delete service example-service

# Delete the deployment
kubectl delete deployment hello-world
```