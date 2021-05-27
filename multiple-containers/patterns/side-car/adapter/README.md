### Adapter plugin
https://kubernetes.io/blog/2015/06/the-distributed-system-toolkit-patterns/#example-3-adapter-containers


#### https://github.com/bmuschko/ckad-prep/blob/master/3-multi-container-pods.md#implementing-the-adapter-pattern

```bash
# create two containers based on the above example
# create a pod yaml
kubectl run app --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c 'while true; do echo "$(date) | $(du -sh ~)" >> /var/logs/diskspace.txt; sleep 5; done;' > adapter.yaml
kubectl apply -f adapter.yaml

# Connect to second container
kubectl exec app -it -c transformer -- /bin/sh

cd /var/logs/
ls -rt
-rw-r--r--    1 root     root          1176 May 27 19:28 diskspace.txt
/var/logs # tail -f diskspace.txt

# delete the pod
kubectl delete po app --grace-period=0 --force
```