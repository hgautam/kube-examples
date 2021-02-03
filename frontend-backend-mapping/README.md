### Front-end to Back-end mapping

In this example, we map front-end microservice to a back-end microservice

[K8s documentations link](https://kubernetes.io/docs/tasks/access-application-cluster/connecting-frontend-backend/)

Primary constructs used:
* Deployment
* Service

```bash
# create backend service hello deployment
kubectl apply -f backend-deployment.yaml

# View information about the backend Deployment
kubectl describe deployment backend

# Create service to access the deployment
kubectl apply -f backend-service.yaml

# View service info
kubectl describe service hello
```