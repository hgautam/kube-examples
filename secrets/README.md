### Using secrets in Kubernetes

* [Documentation reference](https://kubernetes.io/docs/concepts/configuration/secret/)
* [Distribute Credentials Securely Using Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

#### Create a secret called mysecret2 that gets key/value from a file
```bash
# create a file
echo -n admin > username
kubectl create secret generic mysecret2 --from-file=username
```

#### Create an nginx pod that mounts the secret mysecret2 in a volume on path /etc/foo
* [Documentation link](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#create-a-pod-that-has-access-to-the-secret-data-through-a-volume)
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
# apply the pod
kubectl apply -f pod.yaml
# exec into the pod
kubectl exec -it nginx -- /bin/bash
ls /etc/foo
cat username # this should print admin
```
#### Mount the variable 'username' from secret mysecret2 onto a new nginx pod in env variable called 'USERNAME'
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod2.yaml
# apply the pod
kubectl apply -f pod2.yaml

# kubectl exec
kubectl exec -it nginx -- env | grep USERNAME
```
