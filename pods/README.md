### Get a Shell to a Running Container

We will look at the examples of accessing a pod and getting access into a container running inside a pod.
This section shows how to use **kubectl exec** to get a shell to a running container

[Reference Kubernetes Documentation](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

#### Generate yaml
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -n mynamespace -o yaml > pod.yaml
kubectl create -f pod.yaml
```
#### Generate yaml to pass env command
```bash
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- env > envpod.yaml
```

#### Getting access to container terminal inside a POD
```bash
# apply yaml
kubectl apply -f shell-demo.yaml
kubectl get pod shell-demo
# get shell into the container
kubectl exec --stdin --tty shell-demo -- /bin/bash
# writing file inside the container
echo 'Hello shell demo' > /usr/share/nginx/html/index.html
# install curl
apt-get update; apt-get install curl
# verify
root@minikube:/# curl http://localhost/
Hello shell demo
```
#### Running individual commands in a container
```bash
# print env variables on the container
kubectl exec shell-demo -- env
```
#### Opening a shell when a Pod has more than one container
```bash
# If a Pod has more than one container, use --container or -c to specify a container in the kubectl exec command.
kubectl exec -i -t shell-demo --container nginx -- /bin/bash
```
#### Create multiple containers in a Pod
```bash
# apply multi-pod container yaml
# create vs apply: create is used only for creating pods. apply is used for creating other sources like deployment, RS etc
kubectl create -f multi-container.yaml
# execute a command inside a container busybox2 part of pod busybox
kubectl exec -it busybox -c busybox2 -- ls
```