### ConfigMap 
* [Documentation Reference](https://kubernetes.io/docs/concepts/configuration/configmap/)
* [Various ways to configure a configMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
* [kubectl documentation reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-configmap-em-)
* A configMap can be used as env variable inside a Pod
* A configMap can be mounted as a volume inside Pod

#### Create a configMap called 'options' with the value var5=val5. Create a new nginx pod that loads the value from variable 'var5' in an env variable called 'option'
```bash
kubectl create cm options --from-literal=var5=val5
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > config-pod.yaml
kubectl create -f config-pod.yaml
# to validate
kubectl exec -it nginx -- env | grep option
kubectl delete cm options
kubectl delete po nginx
```
### Create a configMap 'anotherone' with values 'var6=val6', 'var7=val7'. Load this configMap as env variables into a new nginx pod
```bash
kubectl create cm anotherone --from-literal=var6=val6 --from-literal=var7=val7
kubectl run --restart=Never nginx --image=nginx -o yaml --dry-run=client > env-pod.yaml
kubectl create -f env-pod.yaml
# to validate
kubectl exec -it nginx -- env 
# to delete
kubectl delete pod nginx
kubectl delete cm anotherone
```
#### Create a configMap 'cmvolume' with values 'var8=val8', 'var9=val9'. Load this as a volume inside an nginx pod on path '/etc/lala'. Create the pod and 'ls' into the '/etc/lala' directory.
```bash
kubectl create cm cmvolume --from-literal=var8=val8 --from-literal=var9=val9
kubectl run nginx --image=nginx --restart=Never -o yaml --dry-run=client > vol-pod.yaml
kubectl create -f vol-pod.yaml
# to validate
kubectl exec -it nginx -- /bin/sh
cd /etc/lala
ls # will show var8 var9
cat var8 # will show val8
# to delete
kubectl delete cm cmvolume
kubectl delete po nginx
```
#### https://github.com/bmuschko/ckad-prep/blob/master/2-configuration.md#configuring-a-pod-to-use-a-configmap

#### Part 1: create a config map and use it in a pod
```bash
# create a config.txt with couple of vars
# create configmap resource
kubectl create configmap db-config --from-file=config.txt
# create a nginx image based pod that will use this configmap
kubectl run backend --image=nginx --restart=Never --dry-run=client -o yaml > backend.yaml
kubectl create -f backend.yaml
# list env to see the env variables injected by configmap
kubectl exec backend -it -- env
```
### Config-map example

[Redis server](https://kubernetes.io/docs/tutorials/configuration/configure-redis-using-configmap/) based on a config map

Make sure only the following files are in the dir:
* redis-config
* redis-pod.yaml
* kustomization.yaml

```bash
# create a Kustomize yaml
cat <<EOF >./kustomization.yaml
configMapGenerator:
- name: example-redis-config
  files:
  - redis-config
EOF

# Add redi pod resource config to the kustomization.yaml
cat <<EOF >>./kustomization.yaml
resources:
- redis-pod.yaml
EOF

# Apply the kustomization directory to create both the ConfigMap and Pod objects
kubectl apply -k .

# Examine the created objects
kubectl get -k .

# Use kubectl exec to enter the pod and run the redis-cli tool to verify that the configuration was correctly applied
kubectl exec -it redis -- redis-cli
127.0.0.1:6379> CONFIG GET maxmemory
127.0.0.1:6379> CONFIG GET maxmemory-policy

# delete resources
kubectl delete pod redis
kubectl delete configmap example-redis-config-dgh9dg555m
```
