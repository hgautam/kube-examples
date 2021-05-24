### Multiple containers on Pods

[Kubernetes Example](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/)

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
