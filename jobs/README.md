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

#### Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute

```bash
kubectl create job busybox --image=busybox --dry-run=client -o yaml -- /bin/sh -c 'while true; do echo hello; sleep 10;done' > job.yaml
vi job.yaml
# add add the following line:
Add job.spec.activeDeadlineSeconds: 30

kubectl apply -f job.yaml
kubectl logs busybox-* -f
kubectl delete job busybox
```
#### Create the same job, but make it run 5 parallel times

```bash
kubectl create job busybox --image=busybox --dry-run=client -o yaml -- /bin/sh -c 'while true; do echo hello; sleep 10;done' > job-parallelism.yaml
vi job-parallelism.yaml
# add the following line:
parallelism: 5

kubectl apply -f job-parallelism.yaml
kubectl logs busybox-* -f
kubectl delete job busybox
```