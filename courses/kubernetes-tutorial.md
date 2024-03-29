# Kubernetes Tutorial

[Docker Tutorial for Beginners - Techworld with Nana](https://youtu.be/X48VuDVv0do)


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

For example, a small cluster may contain:
- 2 master nodes (low on resources)
- 3 worker nodes (higher in resources)


### Minikube and Kubectl

To manage the cluster we use the API Server (master process). We can interact with it by:
- UI: For example one of the dashboards
- API
- CLI: For example kubectl (most features)

Minikube allows use (for testing purposes, for example) to have both master and worker processes on the same machine.


### Kubectl commands

Get status of any component (node, pod, service...)

	kubectl get COMPONENT

Create a certain component

	kubectl create COMPONENT

We don't Create pods, we create Deployments (which is an abstraction above), giving the image:

	kubectl create deployment NAME --image=image [--options]

More complicated deployments will be seen afterwards, as this one gives almost everything to default.

This default configuration also creates a ReplicaSet, leaving the following structure: Container in Pod in ReplicaSet in Deployment. This is visible in the IDs. We just configure the Deployment and K8s manages the layers below.

Edit deployment's configuration (autogenerated on create):

	kubectl edit deployment NAME

After changing the configuration, K8s will automatically manage the layers below (creating new pods and destroying old ones, for example).

For viewing logs:

	kubectl logs POD_NAME

If it has not started yet, one can view the stage in the starting process by:

	kubectl describe POD_NAME

Enter a pod's terminal:

	kubectl exec -it POD_NAME -- /bin/bash

Delete a deployment (will also delete layers below):

	kubectl delete deployment NAME

In reality, we provide a configuration (yaml) to K8s instead of creating/editing the deployment (or other components) from the terminal. We can apply a configuration by using:

	kubectl apply -f CONF_FILE

We can also delete the component from the configuration file with:

	kubectl delete -f CONF_FILE 

K8s automatically creates or updates the deployments to match the configuration given.


### Configuration File

The configuration is devided in 3 parts:

- Metadata: Name, etc.
- Specification: Depending on the kind of component
- Status: **Generated and maintained by K8s**

K8s automatically updates the status based on the real values (from etcd). If it does not match the specification, it makes the necessary changes to make them match.

#### Deployment configuration

As said before, to manage pods we configure the deployment. A deployment's configuration looks like this:

```yaml
apiVersion: x
kind: Deployment
metadata:
    name: x
    labels: ...
# Until here it does not depend on the type
spec:
    replicas: 2
    selector: ...
    template: ...
```

The template defines the configuration of the pod, it has its own metadata and specification (like a configfile within a configfile).


#### Labels and selectors

It exists so the deployment knows which containers depend on it. The container defines a label and the deployment matches those pods with that label.

The deployment can also have a label, which it can be used by a Service (for example) to know which deployments it has to interact with.


#### Ports

Service has:
- port: The external one
- targetPot: The one that matches with the pod

Pod has:
- containerPort: Should match the targetPort

To get the ips assigned to pods:

	kubectl get pod -o wide


### Demo: MongoDB and MongoExpress

Components:

- MongoDB pod with an Internal Service attached
- Mongo Express Pod with an External Service
- ConfigMap to store DB URL
- Secret to store DB User / DB Password

In the deployment, we will reference the values from the ConfigMap and Secret to configure the Mongo Express.

So the data flows like so: Browser -> External Service -> Mongo Express Pod -> Internal Service -> MongoDB.

The configuration files are in the following links: <https://gitlab.com/nanuchi/youtube-tutorial-series/-/tree/master/demo-kubernetes-components>.

To configure a external service we set `type: LoadBalancer` (misleading name, as internal services also act as load balancers)


### Namespaces

To have a rough idea, it is an isolated cluster inside the K8s cluster. There are some by default:

- kube-system: Must not modify, it contains processes for the master node
- kube-public: Contains public facing data (even with no auth)
- kube-node-lease: Contains ionformation about the availability of the nodes
- default: Where everything goes by default

One can create namespaces with `kubectl` and config files, as always.

The idea of having namespaces is to group resources so it's more organized. It should be avoided for small projects.

Some additional ideas to use namespaces:
- To avoid collisions from multiple teams
- To reuse components in Staging/Prod (or AB testing, etc) environments
- Limiting access permissions or permissions

Some considerations:
- Most components cannot be used through namespaces. Service does not, for example
- Some components cannot be inside a namespace (for example volume and node)

The namespace can be assigned in the `metadata` section of the config.

There are some tools for working with kubectl on different namespaces easier: [kubectx](https://github.com/ahmetb/kubectx#installation).


### Ingress

External services can be accessed from outside, but through the node's IP (which is not convenient).

Ingress is a component that behaves similarly to a Reverse Proxy, by forwading outside requests (with URL and TLS too) to internal services.

The DNS has to be configured so that the domain name points to the node running the Ingress Component.

Note that the implementation difference between Internal and External port is:
- Internal: Type ClusterIP (default) and no nodePort (only port and targetPort)
- External: Type LoadBalancer and nodePort, port and targetPort

For Ingress to be able to define the routes it needs an Ingress Controller. There are many options, the one created by K8s is Nginx Ingress Controller. [List of controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)

Depending on the Infra of the Cluster, the ingress implementation can be very different:
- Cloud: Normally there is some solution given by the cloud provider
- Bare Metal: An entrypoint has to be configured, which may not be trivial. Some advice [here](https://kubernetes.github.io/ingress-nginx/deploy/baremetal/).

#### Minikube demo

First we install and run Nginx Ingress Controller:

	minikube addons enable ingress

In the example we make the dashboard accessible from outside the cluster using the Ingress. Its configuration is [here](https://gitlab.com/nanuchi/youtube-tutorial-series/-/blob/master/kubernetes-ingress/dashboard-ingress.yaml).

To test the ingress implementation, we can modify `/etc/hosts` to map the given domain to the Ingress IP.

#### Default backend

If a request is recieved that has no matching route defined, it is sent to the default backend.

We can modify this default backend behaviour, by creating an internal service with the same name.

#### Some more complex use cases

We can forward the requests to different services depending on the given path, for example:

```yaml
...
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /one
        backend:
          serviceName: service-one
          servicePort: 8080
      - path: /two
        backend:
          serviceName: service-two
          servicePort: 8080
```

Or depending on the subdomain:

```yaml
...
spec:
  rules:
  - host: one.example.com
    http:
      paths:
        backend:
          serviceName: service-one
          servicePort: 8080
  - host: two.example.com
    http:
      paths:
        backend:
          serviceName: service-two
          servicePort: 8080
```

And we can also configure TLS with a Secret of type TLS (which has to be in the same namespace as the Ingress):

```yaml
...
spec:
  tls:
  - hosts:
    - example.com
    secretName: example-tls-secret
  rules:
  ...
```


### Helm

The first use is as a Package Manager. A *package* is a Helm Chart, which is a collection of configuration files for certain application.

Public registries are available at Helm Hub or though `helm search NAME`.

Helm can also be used as templating engine. We can insert values from a file called `values.yml` (can be overriden with the `--set` flag), useful for CI/CD. It can also be overriden with another values file (`--values=VALS_FILE` flag)


### Volumes

There are three main components for volumes:
- Persistent volume
- Persistent volume claim
- Storage Class

Storage must be:
- Non-dependant on pod's lifecycle
- Available from all nodes
- Resiliant from cluster's crashes

When defining the storage, we must define the backend (type of storage): Local drive, cloud storage, etc.

Local and remote storages have different use-cases. For example, local should not be used to store a DB's data because:
- It is tied to a specific node
- Does not survive to a cluster crash

The storage must be available before the pods use it. Application pods use a Persistent Volume Claim component to request storage with certain characteristics, and K8s gives the adequate Persistent Volume so the pod can use it.

As a note, Secret and ConfigMap are also volumes, but special as they are managed by K8s.

StorageClass is a third type that allows Persistent Volumes to be created automatically when requested by a PVC.


### Stateful Sets

A component for managing applications that require a state (for example databases).

- Stateless applications are managed by deployments
- Stateful applications are managed by a Stateful Set component (STS)

In stateless applications, no identity has to be tracked as all replicas are identical and interchangable.

However in stateful applications, to avoid data races, an identity is given (and mantained) to each replica. One of the replicas is the master (has write permission), and the rest are slaves.

As each pod mantains its physical data, when Master writes to his own the rest have to also make this update.

The pod state (role in STS, etc) is also stored in the PV, so when the pod is replaced, the new one keeps the same identity.

The pods are numerated in ascending order: 0 (master), 1, 2, 3... (slaves). Deletion and addition of pods keeps the continued ordering. A DNS name is also assigned to each pod (so identity is kept even if the IP is changed).


### Services

To get the IP address of a certain pod:

	kubectl get pod -o wide

#### ClusterIP

Default service type for K8s.

Meaning of service ports:
- servicePort: Port in which the service expects requests to arrive
- targetPort: Port in which selector will forward the request

A single service can have multiple tuples of (servicePort, targetPort) defined. This is called a MultiPort Service, and it requires each tuple to also have a name.

The service knows which pods correspond to him with the `selector` attribute. It matches the pods that have **all** the selector key-value pairs defined on their `labels` attribute.

When a pod matches with a selector, K8s creates an Endpoint object with the name of the service to keep track of all the pods that are connected. Check with `kubectl get endpoints`.

##### Headless Service

Supose we don't want to access a random pod connected to the service, but a particular one. This is common on Stateful Applications, where the pods are not identical.

We use Headless Services, which do not give an IP to the service and the pod's IP is directly returned.

#### NodePort

A service that is accessible from outside through a static port on each worker node. We specify this port with the `nodePort` value.

To connect the node's opened port (nodePort) with the service's opened port (servicePort), a ClusterIP service is creted automatically.

This is useful when we have a pod that we want ot be available in all the nodes, but it is not the most secure approach as it exposes the ports directly to outside the cluster.

In most cases, the LoadBalancer type is preferred (more secure), but it can be useful for quick testing.

#### LoadBalancer

Seen before. It can depend on our cloud provider. We have the attributes:
- nodePort: Port that will be accessible from outside
- port: Port that the service listens to
- targetPort: Port that the service forwards to

We can see that a LoadBalancer is an extension of NodePort, which is an extension of ClusterIP