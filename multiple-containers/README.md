### Multiple containers on Pods

* [Kubernetes Documentation Example](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/)
* [Logging arch - explains container patterns](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

#### Create a Pod with two containers, both with image busybox and command "echo hello; sleep 3600". Connect to the second container and run 'ls'
```bash
# create a single container pod with yaml
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml -- /bin/sh -c "echo hello; sleep 3600" > multi-container.yaml
# modify the yaml and add the followin section
- args:
    - /bin/sh
    - -c
    - echo hello; sleep 3600
    image: busybox
    name: busybox
    resources: {}
# apply the yaml
kubectl apply -f multi-container.yaml

# checking logs of a container
kubectl logs busybox -c busybox

# connect to second container and run ls
kubectl exec -it busybox -c busybox2 -- ls

# you can do some cleanup
kubectl delete po busybox
```
#### **Init-container example:** Create pod with nginx container exposed at port 80. Add a busybox init container which downloads a page using "wget -O /work-dir/index.html http://neverssl.com/online". Make a volume of type emptyDir and mount it in both containers. For the nginx container, mount it on "/usr/share/nginx/html" and for the initcontainer, mount it on "/work-dir".
Empty dir doc reference: https://kubernetes.io/docs/concepts/storage/volumes/#emptydir-configuration-example
```bash
# create a pod
kubectl run nginx --image=nginx --restart=Never --port=80 --dry-run=client -o yaml > init-container.yaml

# make empty dir volume
# add the following snippets to the yaml
volumes:
  - name: cache-volume
    emptyDir: {}
# add init container
# apply the pod
kubectl create -f init-container.yaml

# get the IP
kubectl get po nginx -o wide
# get the IP of the created pod and create a busybox pod and run "wget -O- IP"
kubectl run busybox --image=busybox --restart=Never --rm -- /bin/sh -c "wget -O- 172.17.0.3" 
# you can do some cleanup
kubectl delete po box
```

#### https://github.com/bmuschko/ckad-prep/blob/master/3-multi-container-pods.md#implementing-the-adapter-pattern
