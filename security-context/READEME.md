### Setting security context for Pods

[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

#### Create the YAML for an nginx pod that runs with the user ID 101
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
# add security context by manually editing the pod yaml - pod.yaml for details
```
#### https://github.com/bmuschko/ckad-prep/blob/master/2-configuration.md#creating-a-security-context-for-a-pod
```bash
# Create a Pod named secured that uses the image nginx for a single container. Mount an emptyDir volume to the directory /data/app
kubectl run secured --image=nginx --restart=Never --dry-run=client -o yaml > sec-pod.yaml
# create fs security group 
# create the pod
kubectl create -f sec-pod.yaml
# validate
kubectl exec -it secured -- /bin/sh
# cd /data/app
# ls -lrt
total 0
# touch test.txt
# ls -lrt
total 0
-rw-r--r-- 1 root 3000 0 Jun  7 16:50 test.txt
# delete pod
kubectl delete po secured
```