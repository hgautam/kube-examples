### Using secrets in Kubernetes

[Documentation reference](https://kubernetes.io/docs/concepts/configuration/secret/)
[Distribute Credentials Securely Using Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

#### Create a secret called mysecret2 that gets key/value from a file
```bash
# create a file
echo -n admin > username
kubectl create secret generic mysecret2 --from-file=username
```

#### Create an nginx pod that mounts the secret mysecret2 in a volume on path /etc/foo
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
# apply the pod
kubectl apply -f pod.yaml
# exec into the pod
kubectl exec -it nginx -- /bin/bash
ls /etc/foo
cat username # this should print admin
```