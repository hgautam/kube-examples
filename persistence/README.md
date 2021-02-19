### Persistence in Kubernetes

[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/)
[Lab exercise](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

#### Create busybox pod with two containers, each one will have the image busybox and will run the 'sleep 3600' command. Make both containers mount an emptyDir at '/etc/foo'. Connect to the second busybox, write the first column of '/etc/passwd' file to '/etc/foo/passwd'. Connect to the first busybox and write '/etc/foo/passwd' file to standard output
```bash
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c 'sleep 3600' > pod.yaml
# apply yaml
kubeclt apply -f pod.yaml
# Connect to the second container
kubectl exec -it busybox -c busybox2 -- /bin/sh
```
#### Create a PersistentVolume of 10Gi, called 'myvolume'. Make it have accessMode of 'ReadWriteOnce' and 'ReadWriteMany', storageClassName 'normal', mounted on hostPath '/etc/foo'. Save it on pv.yaml, add it to the cluster. Show the PersistentVolumes that exist on the cluster
```bash
# create pv
kubectl create -f pv.yaml

# get pv
kubectl get pv
```

