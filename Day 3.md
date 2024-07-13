## Clusters in Kubernetes
There are many type of clusters.

### Kind cluster
**Kind** is a tool for running Kubernetes clusters using Docker container "nodes". 

Simple yaml file to create a cluster
```yml
kind: Cluster

apiVersion: kind.x-k8s.io/v1alpha4

nodes:

 - role: control-plane
  image: kindest/node:v1.30.0@sha256:047357ac0cfea04663786a612ba1eaba9702bef25227a794b52890dd8bcd692e

 - role: worker
  image: kindest/node:v1.27.13@sha256:17439fa5b32290e3ead39ead1250dca1d822d94a10d26f1981756cd51b24b9d8
```

Visit [site](https://github.com/kubernetes-sigs/kind/releases) for node images.
**NOTE**: Version of worker node should be lesser than the master node(cluster plane) due to backward compatibility issues

Now run the command ->
```shell
kind create cluster --name day3 --config cluster.yaml
```

## Images

Images are the key building blocks of Containerized Infrastructure. It contains the whole filesystem that the application will use and additional metadata, such as the path to the executable file to run when the image is executed, the ports the application listens on, and other information about the image.

A full image name has the following format and components:
`HOST`: Registry hostname specifies where the image is located.
`PORT NUMBER`: If hostname is present it may optionally be followed by registry port number.
`PATH`

###### Image Tag
After the image name, the optional `TAG` is a custom, human-readable manifest identifier that's typically a specific version or variant of an image.
```
image:tag
```

##### Layers in Image

There are multiple layer in an image. A Docker build consists of a series of ordered build instructions. Each instruction in a Dockerfile roughly translates to an image layer.

More layer more heavy the image will. It's preferred to write docker file so that it's create a image with minimum layers possible.
![img](https://docs.docker.com/build/guide/images/layers.png)

##### Distroless Images
"Distroless" images contain only your application and its runtime dependencies. They do not contain package managers, shells or any other programs you would expect to find in a standard Linux distribution.

These images are very light.

Visit [site](https://github.com/GoogleContainerTools/distroless?tab=readme-ov-file) to read more about distroless images.


## Containers

Containers are technologies that allow the packaging and isolation of applications with their entire runtime environment—all of the files necessary to run. You can also easily move the containerized application between public, private and hybrid cloud environments and data centers.

There are two type of Containers 
 - Stateless 
 - Stateful
 - Init
 - Bebug
 - Distroless, rootless
 - Ephemeral 

## Kubectl

Kubectl is a command line utility for kubernetes.

- Create a resource: `kubectl create -f <file name>`
- Edit a resource: `kubectl edit <resource name>`
- Execute a command in a container: `kubectl exec <pod name> -c <container name> -- <command> <args>`
- Expose a resource as a service: `kubectl expose <resource name>`
- Get information about a resource: `kubectl get <resource type> <resource name>`
- Get logs of a container: `kubectl logs <pod name> -c <container name>`
- Port forward: `kubectl port-forward <resource name> <local port>:<container port>`
- `kubelet get node` return a list of all nodes in the cluster.
- `kubectl describe node <node-name>` return information such as the node’s hostname, external IP address, internal IP address, and capacity (CPU, memory, and maximum number of pods)


## PODs

_**Pods**_  are the smallest deployable units of computing that you can create and manage in Kubernetes. A _Pod_  is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.

_Creating a Pod_
```yml
apiVersion: v1
kind: Pod
metadata:
  name: simple-api
spec:
  containers:
  - name: simple-api
    image: simple-api:1.0.0
    ports:
    - containerPort: 3000
```

**apiVersion**
The apiVersion is used to let Kubernetes know which schema to use when creating a resource. This value is required to let Kubernetes know which version of the schema should be used when creating the resource.

**kind**  
There are many resources kinds handled by Kubernetes, from pods to ingress controllers. This is a required key that lets Kubernetes know what type of resource is being created.

**metadata**  
Metadata is where we provide details about the resource that is being declared.

**spec**  
The spec is the most important section where we declare how our resource must be created. For a Pod, we describe the container image to be used, the name the container will run as, as well as other details, such as ports and environment variables.


**kubeclt command to create a pod**
```bash
kubectl run pod_name --image=image_name
```

**using yaml file**
```bash
kubectl apply -f file_name.yaml
```

**delete a pod**
```bash
kubectl delete pod/pod_name
```


## Exec in the Pods

The "kubectl exec" command **enables you to get inside a running container by opening and accessing its shell**.

```bash
kubectl exec -it pod_name -- command(ex bash)
```

**We don't use exec in production**


### Ephemeral Containers
	
Ephemeral containers are a special type of container that runs temporarily in an existing Pod to accomplish user-initiated actions such as troubleshooting

### Debug containers
The debug command spins up a new container into a running pod. This new container can run as a different user and from any image you choose. Because the debug container runs within the same pod as the container it targets , the isolation between both containers does not need to be absolute. The debug container can share system resources with other containers running in the same pod.