# Replication Controller / Replica Set / Daemon Set

### Liveness

```
kubectl apply -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-rc/pod-liveness.json
$ kubectl get pods
NAME              READY   STATUS    RESTARTS   AGE
spring-boot-web   1/1     Running   2          3m54s

kubectl describe po spring-boot-web

Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  4m53s                  default-scheduler  Successfully assigned default/spring-boot-web to minikube
  Normal   Pulling    4m51s                  kubelet, minikube  Pulling image "rkp201/k8s-spring-app:1.5"
  Normal   Pulled     4m30s                  kubelet, minikube  Successfully pulled image "rkp201/k8s-spring-app:1.5"
  Warning  Unhealthy  2m51s (x2 over 4m21s)  kubelet, minikube  Liveness probe failed: Get http://172.18.0.4:8080/health: dial tcp 172.18.0.4:8080: connect: connection refused
  Normal   Created    90s (x3 over 4m29s)    kubelet, minikube  Created container spring-boot-web
  Normal   Started    90s (x3 over 4m29s)    kubelet, minikube  Started container spring-boot-web
  Warning  Unhealthy  11s (x9 over 3m20s)    kubelet, minikube  Liveness probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing    11s (x3 over 3m)       kubelet, minikube  Container spring-boot-web failed liveness probe, will be restarted
  Normal   Pulled     10s (x3 over 2m59s)    kubelet, minikube  Container image "rkp201/k8s-spring-app:1.5" already present on machine
  
events showing pod is getting restarted due to unhealthy.  
```
