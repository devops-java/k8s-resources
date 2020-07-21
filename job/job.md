### K8S JOB

creating job with java app in the pod
```
$ kubectl apply -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/job/pod-job.json
job.batch/counterjob created
$ kubectl get jobsNAME         COMPLETIONS   DURATION   AGE
counterjob   0/1           5s         5s
$ kubectl get poNAME               READY   STATUS    RESTARTS   AGE
counterjob-lnqv8   1/1     Running   0          12s
```
here our job is running as a pod. so get the pod ip and we will start the counter with api call. we will stop the java program with api call.
this will terminate the pod. 
```
$ kubectl describe pod | grep -i ip
IP:           172.18.0.4
IPs:
  IP:           172.18.0.4
 
$ curl 172.18.0.4:8989/job/RUN
RUNNING$

$ kubectl logs counterjob-lnqv8

above will show the counter logs.
```

now we will terminate the job with api call. we can see the job also will complete.
```kubernetes
$ kubectl get jobs
NAME         COMPLETIONS   DURATION   AGE
counterjob   0/1           6m51s      6m51s
$ curl 172.18.0.4:8989/job/STOP
curl: (52) Empty reply from server
$ kubectl get jobs
NAME         COMPLETIONS   DURATION   AGE
counterjob   0/1           7m9s       7m9s
$ kubectl get jobs
NAME         COMPLETIONS   DURATION   AGE
counterjob   1/1           7m11s      7m13s

$ kubectl get pod
NAME               READY   STATUS      RESTARTS   AGE
counterjob-lnqv8   0/1     Completed   0          7m35s
```
