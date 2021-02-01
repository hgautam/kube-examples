### Get a Shell to a Running Container

We will look at the examples of accessing a pod and getting access into a container running inside a pod.
This section shows how to use **kubectl exec** to get a shell to a running container

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