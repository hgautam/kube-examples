### K8s deployment examples
[Documentation](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

#### Create a deployment with image nginx:1.18.0, called nginx, having 2 replicas, defining port 80 as the port that this container exposes

```bash
# create deployment yaml
kubectl create deploy nginx --image=nginx:1.7.8 --replicas=2 --port=80 -o yaml > deployment.yaml
# apply the yaml
kubectl apply -f deployment.yaml
```
