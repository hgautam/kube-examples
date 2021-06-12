### Persistence in Kubernetes

* [Persistent vol documentation](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
*[How to configure a vol](https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/)
*[How to configure pvc](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

#### Create busybox pod with two containers, each one will have the image busybox and will run the 'sleep 3600' command. Make both containers mount an emptyDir at '/etc/foo'. Connect to the second busybox, write the first column of '/etc/passwd' file to '/etc/foo/passwd'. Connect to the first busybox and write '/etc/foo/passwd' file to standard output
```bash
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c 'sleep 3600' > pod.yaml
# apply yaml
kubeclt apply -f pod.yaml
# Connect to the second container
kubectl exec -it busybox -c busybox2 -- /bin/sh
cat /etc/passwd | cut -f 1 -d ':' > /etc/foo/passwd 
cat /etc/foo/passwd # confirm that stuff has been written successfully
exit
# validate on first container
kubectl exec -it busybox -c busybox -- /bin/sh
mount | grep foo # confirm the mounting
cat /etc/foo/passwd
exit
# clean up
kubectl delete po busybox
```
#### Create a PersistentVolume of 10Gi, called 'myvolume'. Make it have accessMode of 'ReadWriteOnce' and 'ReadWriteMany', storageClassName 'normal', mounted on hostPath '/etc/foo'. Save it on pv.yaml, add it to the cluster. Show the PersistentVolumes that exist on the cluster
```bash
# https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolumeclaim
# create pv
kubectl apply -f pv.yaml

# get pv
kubectl get pv
```

#### Create a PersistentVolumeClaim for this storage class, called mypvc, a request of 4Gi and an accessMode of ReadWriteOnce, with the storageClassName of normal, and save it on pvc.yaml. Create it on the cluster. Show the PersistentVolumeClaims of the cluster.
```bash
# https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolumeclaim
# see the yaml
cat pvc.yaml

# apply the yaml
kubectl apply -f pvc.yaml
```
#### Create a busybox pod with command 'sleep 3600', save it on pod.yaml. Mount the PersistentVolumeClaim to '/etc/foo'. Connect to the 'busybox' pod, and copy the '/etc/passwd' file to '/etc/foo/passwd'
```bash
# https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-pod
# create pod
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c 'sleep 3600' > pod1.yaml

# apply the pod
kubectl apply -f pod1.yaml

# Connect to the pod and copy '/etc/passwd' to '/etc/foo/passwd':
kubectl exec busybox -it -- cp /etc/passwd /etc/foo/passwd

# read the contents
kubectl exec busybox -- cat /etc/foo/passwd
```
#### Create a busybox pod with 'sleep 3600' as arguments. Copy '/etc/passwd' from the pod to your local folder
```bash
kubectl run busybox --image=busybox --restart=Never -- /bin/bash -c 'sleep 3600'

# copy /etc/passwd to local dir
kubectl cp busybox:etc/passwd ./passwd

cat passwd
```
#### https://github.com/bmuschko/ckad-prep/blob/master/7-state-persistence.md#defining-and-mounting-a-persistentvolume
```bash
```