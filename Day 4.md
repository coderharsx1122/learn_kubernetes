# Image Security

Image security  is crucial  part of Kubernetes securtiy  as it can lead to container breakout.
### Trivy (a tool)

Trivy, A Simple and Comprehensive Vulnerability Scanner for Containers and other Artifacts, Suitable for CI is the most popular open source security scanner, reliable, fast, and easy to use. Use Trivy to find vulnerabilities & IaC misconfigurations, SBOM discovery, Cloud scanning, Kubernetes security risks,and more.

```bash
trivy image image_name # list all the potential vulnreability with potenial severity like critical medium high low
```
example to list only critical severity vulnerability 
```bash
trivy image image_name --severity CRITICAL
```

### Kyverno

Kyverno is a policy engine designed for Kubernetes platform engineering teams. It enables security, automation, compliance, and governance using policy-as-code. Kyverno can validate, mutate, generate, and cleanup configurations using Kubernetes admission controls, background scans, and source code respository scans.

I like to call it **POLICY SETTER**

**Kyverno acts like security gurad**

A Kyverno  policy file looks like this:-

```yaml
# GENERATE POLICY
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: add-networkpolicy
spec:
  rules:
  - name: default-deny
    match:
      any:
      - resources:
          kinds:
          - Namespace
    generate:
      apiVersion: networking.k8s.io/v1
      kind: NetworkPolicy
      name: default-deny
      namespace: "{{request.object.metadata.name}}"
      synchronize: true
      data:
        spec:
          # select all pods in the namespace
          podSelector: {}
          # deny all traffic
          policyTypes:
          - Ingress
          - Egress
```

### KubeLinter

KubeLinter analyzes Kubernetes YAML files and Helm charts, and checks them against a variety of best practices, with a focus on production readiness and security. This is to help teams check early and often for security misconfigurations and DevOps best practices.

```bash
# example
kube-linter lint file_name.yaml
```
will return lint errors and list of suggestion to change for production

### Kube bench

Kube-bench is an open-source tool to assess the security of Kubernetes clusters by running checks against the Center for Internet Security [(CIS) Kubernetes benchmark.](https://www.cisecurity.org/benchmark/kubernetes)

### Static Pods

_Static Pods_ are managed directly by the kubelet daemon on a specific node, without the API server observing them. In case if pod get down then kubelet will reboot and restart the pod without asking kube api server.

path for static pods = `/etc/kubernetes/manifests`
path for kubelet config = `/var/lib/kubelet` has config.yaml
#### Daemon set
Daemon set are static pods who runs on multiple nodes.
**Kubeproxy** itself run as daemon set. 

### Init Containers

They are container which run before the application container.
Init containers support all the fields and features of app containers, including resource limits, volumes, and security settings.
If a Pod's init container fails, the kubelet repeatedly restarts that init container until it succeeds.
Init containers run one by one.

Yaml file for init container

```yaml
#test file
apiVersion: v1
kind: pod
metadata:
    name: my-pod
spec:
    containers:
      - name: main-container
        image: hello-world
        command: ["sleep","id"]
    initContainers:
      - name: init-container
        image: google/cloud-sdk:latest
        command: ["sh","-c","gsutil cp gs://bootup-files/policy.yaml /var/policies"]
        volumeMounts:
         - name: shared-data
           mountPath: /var/
        volumes:
         - name: shared-data
           emptyDir: {}
```

```bash
kubectl apply -f file.yaml
```

### Sidecar containers

Sidecar containers came into use in Kubernetes v1.29 beta.  These containers are used to enhance or to extend the functionality of the primary _app container_ by providing additional services, or functionality such as logging, monitoring, security, or data synchronization, without directly altering the primary application code. It does not die unlike init containers.


##### POD CYCLE

1. kubectl -> apiserver -> etcd -> apiserver -> apiservice -> kubelet
2. Init container 
3. Create pods
4. Probes (make sure that application is healthy)
5. Then Traffic

#### POD LIFECYCLE

- Pending
- Succeeded 
- Failed
- Running
- Unknown

#### CONTAINER RUNTIME
- Running
- Waiting
- Terminated

### Termination of A Pod

When we try to delete a pod, kubelet used to send a _SIGTERM_ signal to container runtime for the deletion of the pod within 30s grace time , SIGTERM is **a signal that is sent to a process to request that it gracefully terminate**. If it doesn't shut down in time, the process is then killed through SIGKILL .

### Runtime class

Kata is the most common runtime. **Kata Containers is an open source container runtime, with lightweight virtual machines that feel and perform like containers, but provide stronger workload isolation using hardware virtualization technology as a second layer of defense**

