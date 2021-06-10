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
#### Get a shell to the running container and print out the token of the service account.
```bash
kubectl exec -it nginx -- /bin/sh
cat /var/run/secrets/kubernetes.io/serviceaccount/token

# clean up
kubectl delete po nginx
kubectl delete sa myusers
```