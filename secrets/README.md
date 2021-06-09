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
* [Documentation link](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#define-a-container-environment-variable-with-data-from-a-single-secret)
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod2.yaml
# apply the pod
kubectl apply -f pod2.yaml

# kubectl exec
kubectl exec -it nginx -- env | grep USERNAME
```
#### https://github.com/bmuschko/ckad-prep/blob/master/2-configuration.md#configuring-a-pod-to-use-a-secret

```bash
# Create a new Secret named db-credentials with the key/value pair db-password=passwd
kubectl create secret generic db-credentials --from-literal=db-password=passwd

# Create a Pod named backend that defines uses the Secret as environment variable named DB_PASSWORD and runs the container with the image nginx
kubectl run backend --image=nginx --restart=Never --dry-run=client -o yaml > backend.yaml

# apply the yaml
kubectl apply -f backend.yaml

# Shell into the Pod and print out the created environment variables. You should find DB_PASSWORD variable
kubectl exec -it backend -- env

# delete pod and secret
kubectl delete po backend
kubectl delete secret db-credentials
```