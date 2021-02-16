### Kubernetes Service accounts

[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

#### Create a new serviceaccount called 'myuser'
```bash
kubectl create sa myuser
```

#### Create an nginx pod that uses 'myuser' as a service account
```bash
kubectl run nginx --image=nginx --restart=Never --serviceaccount=myuser --dry-run=client -o yaml > pod.yaml

# apply pod
kubectl apply -f pod.yaml
```