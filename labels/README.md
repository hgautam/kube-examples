### Labels and Annotations

* [Kube documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)

#### Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1

```bash
kubectl run nginx1 --image=nginx --label=app=v1 --restart=Never --dry-run=client -o yaml 
```
