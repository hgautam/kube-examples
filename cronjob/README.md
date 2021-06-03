### Cronjobs in Kubernetes

Based on [Kubernetes Documentation](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)


#### Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output
```bash
kubectl create cronjob busybox --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'
kubectl get cj
kubectl get po busybox-*
kubectl delete cj busybox
```