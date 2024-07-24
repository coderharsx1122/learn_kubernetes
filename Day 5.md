## Labels 

Labels are key value pairs that are attached to objects such as Pods.
Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system. If you don't name label then by default the key will be **_run_** and value will be **_name of the pod_**.

 For example, here's a manifest for a Pod that has two labels `environment: production` and `app: nginx`:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: label-demo
  labels:
    environment: production
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```


Command to get labels of the pods

```bash
kubectl get pods --show-labels
```
output:
```bash
NAME       READY   STATUS    RESTARTS      AGE     LABELS
pod_name   1/1     Running   1 (59s ago)   3d10h   run=pods
```

Command to add labels
```bash
kubectl label pod -l old=label new=label
```

### ReplicaSet

A ReplicaSet (RS) in Kubernetes is a resource that ensures a specified number of identical pod replicas are running at any given time within a cluster.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  replicas: 3
  selector: 
     app: nginx
  template:
     metadata:
        name: nginx
        labels: 
           app: nginx
     spec:
         containers:
            - name: nginx
              image: nginx:1.14.2
              ports:
                - containerPort: 80
```


When scaling down, the ReplicaSet controller chooses which pods to delete by sorting the available pods to prioritize scaling down pods on following general algorithm:

 1. Pending(and unschedulable) pods are scaled down first
 2. If `controller.kubernetes.io/pod-deletion-cost` annotation is set, then the pod with the lower value will come first.
 3. Pods on nodes with more replicas come before pods on nodes with fewer replicas.
 4. If the pods creation times differ, the pod that was created more recently comes before the older pod.

