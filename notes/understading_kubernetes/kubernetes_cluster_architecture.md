# Kubernetes Cluster Architecture

## Core concepts

### The Master and Worker

![The Master and Worker][master_worker]

#### Master (Control Plane) components
The master node can be replicated for high availability setup.
* API Server
  * Commucation hub for all cluster components
  * Exposes the Kubernetes API
* Scheduler
  * Responsible for scheduling workloads to a worker node dependent on resource requirements, hardware constraints, etc.
* Controller Manager
  * Maintains cluster like node failure, replication of components, maintaining correct amount of pods, ets.
* etcd
  * Data store for cluster configuration. This should be backed up

OBS: The master node will not be running any pods and be responsible for running the applications.

#### Worker nodes
* kubelet
  * Responsible for running and managing the containers on the nodes and talking to the API server
  * Pulling required images
* kube-proxy (service-proxy)
  * Load balanaces traffic between application components
* container runtime
  * The program responsible for running your containers (Docker, rkt or containerd)

![Application running on Kubernetes][app_running]

#### Command reference

```bash
kubectl get nodes # List all nodes in cluster

kubectl get pods --all-namespaces # List pods in all namespaces

kubectl get pods --all-namespaces -o wide # List all the pods in the cluster in detail

kubectl get namespaces # Show all namespaces

# Create a nginx pod
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF

kubectl describe pod nginx # All details about the pod nginx

kubectl delete pods nginx # Delete the pod nginx
```

[master_worker]: images/master_and_worker.png
[app_running]: images/app_running_on_kubernetes.png