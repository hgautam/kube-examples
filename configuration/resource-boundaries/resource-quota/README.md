### Resouce quota examples

[Documentation reference](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/)

* [Command reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-quota-em-) 

#### How to create a resource quota in a namespace
```bash
# enable metrics server
minikube addons eanble metrics-server
# create namespace
kubectl create ns myspace
# create resource quota
kubectl create quota my-quota --hard=requests.cpu=1,requests.memory=1Gi,limits.cpu=2,limits.memory=2Gi --namespace myspace \
--dry-run=client -o yaml > my-quota.yaml

# apply resouce quota
kubectl apply -f my-quota.yaml -n myspace
```
#### Another resourcequota example
```bash
# create a namespace rq-demo
kubectl create ns rq-demo
# Create a resource quota named apps
kubectl create quota apps --hard=pods=2,requests.cpu=2,requests.memory=500m --dry-run=client -o yaml > rq.yaml
# apply the quota
kubectl create -f rq.yaml -n rq-demo
# Create a new Pod that exceeds the limits of the resource quota requirements
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
# add high resource requirements
# https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
# apply the pod
kubectl apply -f pod.yaml -n rq-demo
# should result to similar error given below:
Error from server (Forbidden): error when creating "pod.yaml": pods "nginx" is forbidden: exceeded quota: apps, requested: requests.memory=512Mi, used: requests.memory=0, limited: requests.memory=500m
# fix the error by decresing pod memory and re-run pod apply
kubectl apply -f pod.yaml -n rq-demo
# clean up
kubectl delete -n rq-demo
```

