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

In HA environment, multiple master node exists and hence multiple etcd. 
In this case the etcd services in each node must aware about each other.
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

kube-apiserver aware about etcd server host details with the key etcd-servers.kubernets
with kubeadm: kubectl get po -n kube-system will show api-server running as a pod.
apiserver yaml can be found at /etc/kubernetes/manifest/kube-apiserver.yaml

none-kubeadm setup: /etc/systemd/system/kube-apiserver.service

running process of kube-apiserver, grep in ps -aux.

kube controller-manager manages the controllers. a process that monitors the state of system or cluster by using controller(s).
node controller check the status of codes with heartbeat from nodes in every 5 seconds.
replication comntroller monitor the status of number of pods running in cluster.
Deployment Controller, Namespace Controller, Service Controller, Job Controller any many more.
what are the different types of controller availabel in k8s?
kube controller manager can be installed as binary.
see options at /etc/systemd/system/kube-controller-manager.service
kube-controller-manager-master is the pod run by kubeadm(in) tool under the namespace kube-system.
see the options at /etc/kubernets/manifest/kube-controller-manager.yaml
running process ps -aux | grep kube-controller-manager


kube-scheduler decides which pod should go into which node but doesnt places the pod into a node.
kubelet creates the pod on the node.
pods may have diffrenet resource requests or infra needs. By looking at these kube-scheulder chooses appropriate node for the pod.
kube-scheduler can be downloaded as binary and run as a service.
see options at /etc/systemd/system/kube-scheduler.service
with kubeadm kube-scheduler at /etc/kubernetes/manifest/kube-scheduler.yaml
running process ps -aux | grep kube-scheduler


kubelet runs in every worker node. it get request from api-server to onboard the pod.
registers node into the cluster, pull image, run it.
monitor the pods and update the same to api-serevr regularly.
kubelet must need to install on the worker node manually with binaries and run as service. by default kubeadm never installs kubelet.
ps -aux | grep kubelet

kube proxy runs on each node & create appropriate rules (ex: iptable rules in each node) for the newly created service.
pods can communicate with each other in the same cluster over the network.
kube-proxy download as binaries & run as a service.
kubeadm tool installs kube-proxy in each node as daemonset.
kubectl get pod -n kube-system
kubectl get daemonset -n kube-system

</pre>
