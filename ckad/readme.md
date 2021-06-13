<pre>
Download etcd binary and run as a service.
etcd listens on port 2379.
./etcd
./etcdctl set k1 v1
./etcdctl get k1
node, pod , config, secrets, accounts, roles, bindings & others.
kubectl get command gives from etcd.
kubeadm for cluster setup adds etcd by default.
manual cluster setup required etcd setup.
etcd setup remember now advertise-client-url where the etcd listens with port 2379.

with kubeadm:
kubectl get pods -n kube-system
we can see etcd runnign as a pod etcd-master.
list all keys of etcd
kubectl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only
root directory in k8s is registry. under that minions, pods, replicaset, roles, secrets.

In HA environment, multiple master node exists and hence multiple etcd. In this case the etcd services in each node must aware about each other.
To do so we specifify host details of each etcd in intial-cluster-controller.

For example ETCDCTL version 2 supports the following commands:

etcdctl backup
etcdctl cluster-health
etcdctl mk
etcdctl mkdir
etcdctl set

Whereas the commands are different in version 3

etcdctl snapshot save 
etcdctl endpoint health
etcdctl get
etcdctl put

To set the right version of API set the environment variable ETCDCTL_API command

export ETCDCTL_API=3

The master node contains API-SERVER, ETCD, SCHEDULER, CONTROLLER-MANAGER.
The worker node has KUBELET, CONTAINER-RUNTIME, KUBE PROXY.

API SERVER: authenticate user, validate request, interact with etcd, 
scheduler identifies the proper node for pod request and talk with api-server about the node. 
Now api-server delegates to the kubelet of worker node.

kube-apiserver also available as binaries. run it as service in master node.
</pre>
