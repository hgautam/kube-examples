### K8s deployment examples
* [Documentation](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [kubectl reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-deployment-em-)

#### Create a deployment with image nginx:1.7.8, called nginx, having 2 replicas, defining port 80 as the port that this container exposes

```bash
# create deployment yaml
kubectl create deploy nginx --image=nginx:1.7.8 --replicas=2 --port=80 -o yaml > deployment.yaml
# apply the yaml
kubectl apply -f deployment.yaml

# access the nginx from one of the pods
kubectl get po -o wide # this will print the ip

# create a busybox pod and run wget on it against nginx pod
# delete the pod on the completion
kubectl run busybox --image=busybox --restart=Never --rm -it -- wget -O- http://172.17.0.4

# Scale the deployment to 5 replicas
kubectl scale deploy nginx --replicas=5

# Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%
# you would need metrics server enabled for this to work properly
kubectl autoscale deploy nginx --min=5 --max=10 --cpu-percent=80
kubectl get hpa 

# clean up the env
kubectl delete deploy nginx
kubectl delete hpa nginx
```

