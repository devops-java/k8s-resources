# k8s-resources

### End Points
```
Below are the container endpoints
http://localhost:8080/health
http://localhost:8080/host
```

### Manual Start A Pod (without using Replication Container)
```
kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-manual.yaml
kubectl get pods
curl http://podIp:8080/health
curl http://podId:8080/host

kubectl describe po podName -o yaml
kubectl describe po podName -o json

kubectl get po -o wide
```
