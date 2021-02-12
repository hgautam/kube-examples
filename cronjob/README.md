### Cronjobs in Kubernetes

Based on [Kubernetes Documentation](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)

```bash
kubectl create cronjob busybox --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'
```