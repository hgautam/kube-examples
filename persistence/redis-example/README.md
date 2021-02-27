### Redis with local directory

Local node directory mounted as volume 

```bash
# apply redis pod
kubectl apply -f redis.yaml

# check the pod status
kubectl get pod redis -w

# check for the mounted directory inside the pod
kubectl exec -it redis -- bash
root@redis:/data# ls -l /data/redis/
total 0

# create a test file inside /data/redis dir
root@redis:/data# echo hello > /data/redis/test-file
root@redis:/data# ls -l /data/redis/
total 4
-rw-r--r-- 1 root root 6 Feb 27 18:57 test-file

# install ps package
apt-get update
apt-get install procps

# local redis-server process
ps -aux | grep redis

# kill the redis-server
kill <pid>
kill 1

# the container will be restarted because of the default policy of restart always
# lets check back and see if test file is still there
kubectl exec -it redis -- /bin/bash -c 'cat /data/redis/test-file'
hello
```