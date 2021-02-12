### Config-map example

[Redis server](https://kubernetes.io/docs/tutorials/configuration/configure-redis-using-configmap/) based on a config map

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