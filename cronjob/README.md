### Cronjobs in Kubernetes

* Based on [Kubernetes Documentation](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)
* [Kubectl reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-cronjob-em-)


#### Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output
```bash
kubectl create cronjob busybox --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'
kubectl get cj
kubectl get po busybox-*
kubectl delete cj busybox
```
#### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it takes more than 17 seconds to start execution after its schedule.

```bash
kubectl create cronjob cjob --image=busybox --restart=Never --dry-run=client --schedule="* * * * *" -o yaml -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster' > cronjob.yaml
vi cronjob.yaml
# Add cronjob.spec.jobTemplate.spec.activeDeadlineSeconds=17

kubectl apply -f cronjob.yaml
kubectl get cj
kubectl get po
kubectl logs cjob-1622759940-qqszd
kubectl delete cj cjob
```