### K8s deployment examples
[Documentation](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
Based on examples in Kubernetes documentation

```bash
# create deployment yaml
kubectl create deploy nginx --image=nginx:1.7.8 --replicas=2 --port=80 -o yaml > deployment.yaml
# apply the yaml
kubectl apply -f deployment.yaml
```
