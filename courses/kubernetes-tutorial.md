# Kubernetes Tutorial

## Intro to K8s

Kubernetes does Container orchestration.

### Main K8s components

About pods and nodes:
- Node: Server which containers pods
- Pod: Abstract concept in top of container running on an isolated environment
- Each pod has its own IP address, so pods can communicate to each other
- When a pod dies and is restarted, a new IP will be assigned to it

To solve the last point, we introduce the Service:
- Permanent IP address that is attached to a pod
- Load balancing between nodes

Services can be:
- External: Accessible out of the node
- Internal: Accessible only in the node

But connecting to external services through Node's IP and Service's port is not feasable, thus we introduce the Ingress:
- Recieves the requests from outside
- Forwards the requests to the internal services

To introduce environment values to the pods (for example URLs to other pods) we can use ConfigMap. To store confidential data, we instead use the Secret component (is base64 encoded).

To store data, we use Volumes, which keep the data even after the pod's lifecicle. It can be inside or outside of the node.

To replicate pods, we create a Deployment for the pod. Thus we work with deployments, declaring the number of replicas in its configuration.

To manage Deployments for Stateful apps (like DB), we use a Stateful set. This will ensure that there won't be data inconsistencies. It may be difficult, so sometimes DB's are directly out of the cluster.

### K8s Arquitecture

Every node (or worker node) has 3 processes running:
- Container runtime: For example Docker
- Kubelet: Process that manages the pods using the container runtime
- Kube proxy: Forwards the requests between pods

To model the arquitecture and make the behavioural decisions for the worker nodes, we use the Master Node.

Every Master Node has:
- API Server: The API that we interact with to configure the cluster
- Scheduler: The program that assigns a new pod to a node and tells the Kubelet to start it
- Controller Manager: Detect changes in clusters (like a pod dying) and tries to recover the previous state (by requesting the Schedule, for example)
- etcd: Key-value storage of the cluster's state. Gets requests from the Scheduler to make decisions, for example

Due to the importance of the master node, it is replicated itself for redundancy.
