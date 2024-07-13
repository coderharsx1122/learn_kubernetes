# Kubernetes Architecture

Kubernetes uses gRPC and REST for communication
## HTTP 1.1 vs HTTP2 and gRPC

HTTP 1.1
  - JSON/XML based
  - Not suited for multiplexing
  
gRPC 
   -  HTTP2 based 

### Components in Kubernetes

There is two main component in Kubernetes

- #### Control Plane (Master Node)
- #### Worker Plane (Worker Node)


### Control Plane
   Control plane component:
  1. Kube-api-server 
  2. etcd
  3. kube scheduler
  4. kube control manager
  5. cloud control manager

#### Kube API server
  It's the most important component in the Kubernetes, If it goes down whole cluster will go down.

**Functions of Kube API Server**

- **Authentication** : Used to authenticate request using bearer token or certificates

- **Authorization** : It monitors RBAC(Role based access control), It monitors whether you have permission or not to create a cluster.

- **Admission Control** : There are two admission controller -> Validating admission controller, Mutating admission controller. Admission controllers may be _validating_, _mutating_, or both. **Mutating controllers** may modify objects related to the requests they admit; validating controllers may not. Admission controllers limit requests to create, delete, modify objects.

-   **Watch for update** :  Kubernetes API server provides a watch endpoint that allows you to watch for updates to resources in real-time.

#### ETCD

ETCD is a nosql database. It's a distributed key-value store that provides a reliable way to store data. When it comes to Kubernetes, etcd reliably stores the configuration data of the Kubernetes cluster, representing the state of the cluster (what nodes exist in the cluster, what pods should be running, which nodes they are running on, and a whole lot more) at any given point of time.

Etcd is written in Go and uses the Raft consensus algorithm to manage a highly-available replicated log.

**ETCD Election** : etcd election refers to the process by which etcd nodes in a cluster elect a leader node to manage the cluster.
 - Election Timeout : The time a node waits before starting a new election after the current leader's failure.

 - Heartbeat Timeout : The time interval between heartbeats sent by the leader to followers so that follower knows leader is working fine.

 **Visti this [site](https://thesecretlivesofdata.com/raft/) to visualize how these election works.**
 
#### Kube Scheduler

Kube scheduler is a process which assign Pods to Nodes. Scheduler decides the best Node for each Pods in the scheduling queue according to constraints and available resources.

#### Kube Controller Manager

The Kubernetes controller manager is daemon(a continuous process which runs in background) that  embeds the core control loops shipped with Kubernetes. In Kubernetes, a controller is a control loop that watches the shared  state of a cluster through the apiserver and makes changes attempting to move the current state towards the desired state.

### Worker Node

Worker Node component
1. Kubelet
2. Kube-proxy

#### Kube-Proxy
Kube proxy runs as a **DaemonSet**. Kube proxy is installed on every worker node. Kube Proxy enables communication between pods and the outside world. It acts as a network proxy and load balancer, allowing incoming traffic to reach pods and ensuring that traffic is routed to the correct pod.

#### Kubelet 

Kubelet is essential component of Kubernetes that is responsible for managing individual nodes in a cluster. Kubelet is used to manage lifecycle of containers and pods. It communicates with Kube API server.

Kubelet  
   -> Calls CRI to take care of the container.
   -> CRI Configure CGroups
   -> Configure Pod's network using CNI
   -> Pull Image
   -> Start the application
   -> Run the application


## CNI vs CRI vs CSI

In early days Kubernetes eco system was in-tree means everything was fully hard coded and companies using Kubernetes was very much dependent on Kubernetes new release to make changes in their products. 
To solve this problem Kubernetes developer created these three interfaces. Which makes it easier to integrate with multiple technology.

### CNI (Container Network Interface)
CNI was created to provide a standard way for container orchestration systems to configure and manage container networks. Before CNI, each container runtime had its own network configuration mechanism, which made it difficult for container orchestration systems to work with multiple container runtimes.

### CRI (Container runtime interface)
CRI was created to provide a standard way for container orchestration systems to interact with container runtimes.Before CRI, container orchestration systems were tightly coupled with specific container runtimes, such as Docker. CRI defines a common interface for container runtimes to provide information about the containers they manage, making it easier for container orchestration systems to work with multiple container runtimes.
**CRI used to call runc(low level container runtime) then runc  spawn and run containers according to the OCI (Open Container Initiative) specification.**

### CSI (Container Storage Interface)
Before CSI, each storage system had its own interface for container orchestration systems to interact with, which made it difficult to integrate with different storage systems. CSI defines a common interface for storage systems to provide storage services to container orchestration systems, making it easier to integrate with different storage systems.

