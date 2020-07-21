## service 

### communicating between service and rc
3 replicas and 1 svc
kubectl apply -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/service/spring-web-app-rc.json
kubectl appyy -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/service/spring-web-svc.json


$ kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
spring-web-rc-2vlxv   1/1     Running   0          4m20s
spring-web-rc-6j4wd   1/1     Running   0          4m20s
spring-web-rc-dxcjw   1/1     Running   0          4m20s


kubectl get svc
NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes           ClusterIP   10.96.0.1      <none>        443/TCP   62m
spring-web-app-svc   ClusterIP   10.96.220.79   <none>        80/TCP    3m13s

radomly connecting to one of the pod on svc port:80
$ curl 10.96.220.79:80/host
hostname: spring-web-rc-2vlxv$ curl 10.96.220.79:80/host
hostname: spring-web-rc-2vlxv$ curl 10.96.220.79:80/host
hostname: spring-web-rc-dxcjw$ curl 10.96.220.79:80/host
hostname: spring-web-rc-6j4wd$


