### Kubernetes Job resources

Examples are based on Kubernetes [Documentation](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

#### Create a job named pi with image perl that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"

```bash
kubectl create job pi --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'
kubectl get job
kubectl get po
kubectl logs po-*
kubectl delete job pi
```