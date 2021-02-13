### Config-map example

[Redis server](https://kubernetes.io/docs/tutorials/configuration/configure-redis-using-configmap/) based on a config map

Make sure only the following files are in the dir:
redis-config
redis-pod.yaml
kustomization.yaml

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
#### Pod that uses configmap from env
```bash
kubectl create cm options --from-literal=var5=val5
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > config-pod.yaml
kubectl create -f config-pod.yaml
# to validate
kubectl exec -it nginx -- env | grep option
```
#### Another variant of Pod that uses configmap
```bash
kubectl create cm anotherone --from-literal=var6=val6 --from-literal=var7=val7
kubectl run --restart=Never nginx --image=nginx -o yaml --dry-run=client > env-pod.yaml
kubectl create -f pod.yaml
kubectl exec -it nginx -- env 
kubectl delete pod nginx
kubectl delete cm anotherone
```
