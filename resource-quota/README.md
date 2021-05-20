### Resouce quota examples

[Documentation reference](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/)


* How to create a resource quota
```bash
# create namespace
kubectl create ns myspace
# create resource quota
kubectl create quota my-quota --hard=requests.cpu=1,requests.memory=1Gi,limits.cpu=2,limits.memory=2Gi --namespace myspace \
--dry-run=client -o yaml > my-quota.yaml

# apply resouce quota
kubectl apply -f my-quota.yaml
```
