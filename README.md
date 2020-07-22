# k8s-resources
### Repository

```
https://github.com/devops-java/spring-docker-image-app
```
### End Points
```
Below are the container endpoints
http://localhost:8080/health
http://localhost:8080/host
```

### Manual Start A Pod (without using Replication Container)
```
When pod killed it wont be restarted by 
kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-manual.yaml
kubectl get pods
curl http://podIp:8080/health
curl http://podId:8080/host

kubectl describe po podName -o yaml
kubectl describe po podName -o json

kubectl get po -o wide
```
### Pod With Labels

```
Labels used to filter the pod.

kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-manual-v1.yaml
kubectl get pods

kubectl get po -l env=design
kubectl get po -l 'env'
kubectl get po -l '!env'

framekwork!=spring to select pods with the framework label with any value other than spring
env in (prod,devel) to select pods with the env label set to either prod or devel
env notin (prod,devel) to select pods with the env label set to any value other than prod or devel

kubectl label po spring-boot-web env=debug --overwrite
kubectl get po -l env=design
kubectl get po -l env!=design
```
### Node Selector Labels
```
kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-manual-v2.yaml
It selects the node which is having label prod="true"
```

### Pod Annotations
```
additional metadata for pod.
not used for filtering. it is a metadata read by programs.
max size is 256kb.
kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-manual-v3.yaml
kubectl get po podName -o yaml
kubectl describe po podName

describe also shows list of events that the container did inside the pod. for example image pull.
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m23s  default-scheduler  Successfully assigned default/spring-boot-web to minikube
  Normal  Pulling    2m22s  kubelet, minikube  Pulling image "rkp201/k8s-spring-app:1.3"
  Normal  Pulled     2m15s  kubelet, minikube  Successfully pulled image "rkp201/k8s-spring-app:1.3"
  Normal  Created    2m14s  kubelet, minikube  Created container spring-boot-web
  Normal  Started    2m14s  kubelet, minikube  Started container spring-boot-web
```
### Namespace (Grouping pods by namespace)
```
We can filter pods by labels but remembering labels is not easy.
So use namespace.

$ kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/namespace.json
namespace/development created
$ kubectl get ns
NAME              STATUS   AGE
default           Active   14m
development       Active   5s
kube-node-lease   Active   15m
kube-public       Active   15m
kube-system       Active   15m

declarative approach:
kubectl create --namespace=development -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-manual-v3.yaml

$ kubectl get po -n development
NAME              READY   STATUS    RESTARTS   AGE
spring-boot-web   1/1     Running   0          13s

deleting the pod with namespace:

$ kubectl delete ns development
namespace "development" deleted
$ kubectl get po -n development
No resources found in development namespace.
$ kubectl get pods
No resources found in default namespace.

descriptor approach:
Here the namespace is added as part of the metadata.
kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/namespace.json
kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-namespace.yaml

You can have two pods with the same name. 
One is in the default namespace, and the other is in your custom-namespace.

When listing, describing, modifying, or deleting objects in other namespaces, 
you need to pass the --namespace (or -n) flag to kubectl. If you don’t specify the namespace, 
kubectl performs the action in the default namespace configured in the current kubectl context. 
The current context’s namespace and the current context 
itself can be changed through kubectl config commands.

if a pod in namespace foo knows the IP address of a pod in namespace bar, 
there is nothing preventing it from sending traffic, such as HTTP requests, to the other pod.

```

### Deleting Pods
```
Use either of the approach:
kubectl delete po podName -n namespaceName
kubectl delete po -l env=design
kubectl delete ns development
```

### Creating Pod With JSON
```
kubectl create -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-manual.json
kubectl get pods
