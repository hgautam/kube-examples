### Liveness and readiness probes
[Kubernetes documentation reference](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

Kubernetes supports three mechanisms for implementing liveness and readiness probes: 
1. running a command inside a container
2. making an HTTP request against a container
3. opening a TCP socket against a container.

#### Create an nginx pod with a liveness probe that just runs the command 'ls'
```bash
# Create pod yaml
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml

# create pod
kubectl apply -f pod.yaml

# validate
kubectl describe po nginx | grep -i liveness

# clean up
kubectl delete -f pod.yaml
```
#### Useful command to describe
```bash
kubectl explain pod.spec.containers.livenessProbe # get the exact names
```
#### Create an nginx pod (that includes port 80) with an HTTP readinessProbe on path '/' on port 80. Again, run it, check the readinessProbe
```bash
# create yaml
kubectl run nginx --image=nginx --restart=Never --port=80 --dry-run=client -o yaml > pod2.yaml

# apply yaml
kubectl apply -f pod2.yaml

# clean up
kubectl delete -f pod2.yaml
```
#### https://github.com/bmuschko/ckad-prep/blob/master/4-observability.md#defining-a-pods-readiness-and-liveness-probe
```bash
# Create a new Pod named hello with the image bonomat/nodejs-hello-world that exposes the port 3000. Provide the name nodejs-port for the container port.
kubectl run hello --image=bonomat/nodejs-hello-world --port=3000 --restart=Never --dry-run=client -o  yaml > pod3.yaml
# Add a Readiness Probe that checks the URL path / on the port referenced with the name nodejs-port after a 2 seconds delay. You do not have to define the period interval.
# Add a Liveness Probe that verifies that the app is up and running every 8 seconds by checking the URL path / on the port referenced with the name nodejs-port. The probe should start with a 5 seconds delay.
# apply the pod
kubectl apply -f pod3.yaml
# Shell into container and curl localhost:3000.
kubectl exec -it hello -- /bin/sh
curl http://localhost:3000
# check the logs of this pod
kubectl logs hello
Magic happens on port 3000
# check liveness events
kubectl get event
# delete pod
kubectl delete po hello
```