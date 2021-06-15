### Setting security context for Pods

[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

Security context defines privilege and access control settings for a pod or a container. And it includes a couple of options. These are Discretionary Access Control, which is about permissions that are used to access an object and Security Enhanced Linux or SE Linux which is where security labels can be applied. Notice that Security Enhanced Linux or SE Linux is something that's not available on all Linux distributions. Also running as a privileged or an unprivileged user or using the Linux capabilities which are advanced features that are exposing access to specific parts of the Linux operating system. And as an alternative to SE Linux there is an AppArmor and there is AllowPrivilegeEscalation which controls if a process can gain more privileges than its parent processes

#### Create the YAML for an nginx pod that runs with the user ID 101
```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
# add security context by manually editing the pod yaml - see pod.yaml for details
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
#### Create the YAML for an nginx pod that has the capabilities "NET_ADMIN", "SYS_TIME" added on its single container
```bash
# create pod yaml
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > sec-cap.yaml
# edit the yaml to add security context capabilities
```