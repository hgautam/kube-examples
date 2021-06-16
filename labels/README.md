### Labels and Annotations

* [Kube documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)
* [kubectl reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#label)

#### Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1

```bash
kubectl run nginx1 --image=nginx --labels=app=v1 --restart=Never --dry-run=client -o yaml
kubectl run nginx2 --image=nginx --labels=app=v1 --restart=Never --dry-run=client -o yaml
kubectl run nginx3 --image=nginx --labels=app=v1 --restart=Never --dry-run=client -o yaml
```
**Labels are applied at the POD level**

#### change a label
```bash
kubectl edit pod nginx1
# change label name to v2
kubectl get po nginx1 --show-labels
# to display new label
```
