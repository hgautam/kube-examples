### Using secrets in Kubernetes

[Documentation reference](https://kubernetes.io/docs/concepts/configuration/secret/)
[Distribute Credentials Securely Using Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

#### Create a secret called mysecret2 that gets key/value from a file
```bash
# create a file
echo -n admin > username
kubectl create secret generic mysecret2 --from-file=username
```