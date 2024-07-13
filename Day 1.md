# Introduction to Kubernetes 

## YAML

When you transfer data from one place to another place, so you can't directly transfer in form of data structures due to latency issue. Data get converted into bytes stream in order to transfer over the network.
Data --(Serialization)--> Byte Stream --(Deserialization)--> Data

**So YAML is human readable Serialization and Deserialization language** **used for writing configuration files**

**YAML is case sensitive**

## Linux for Kubernetes 
In Linux everything is a file.

Some useful linux commands for Kubernetes 

-  `ls` uses to list all files and directory in a folder
-  `ls -a`  uses to list all files and directory in a folder including hidden files and folders
-  `ls -ll`  list long list 
-  `touch` used to create file
-  `nano or vim` is code editor within terminal
-  `ps aux` lists all running process
-  `pstree` gives  hierarchical view or running processes 
-   `ls -lh`
-  `echo word | base64` will encode your word text in base64
-  `echo base64_encoded | base64 --decode` decodes the encoded value from base64 to text
-  `head file_name` will show first top 10 line of the file
-  `tail file_name` will show last top 10 line of the file
-  `tcpdump`  allows you to capture and analyze network traffic
-  `tcpdump -c 10` will show you only 10 packets on network traffic
-  `ctr`
-  `crictl` is a command-line interface for CRI-compatible container runtimes. You can use it to inspect and debug container runtimes and applications on a Kubernetes node.
-  `crictl pods` list all pods 
-  `crictl ps -a` list all containers 
   Visit [site](https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/) for more crictl commands
-  `journalctl`  journalctl command is used to manage and query the systemd journal, which is a centralized log management system in Linux. 
    **systemd is stored in /etc/systemd**
-  `journalctl --since yesterday` show logs generated since yesterday


## What is Kubernetes

Kubernetes is like a controller set which use to control  things. It is mainly used to automate the deployment, scaling and management of containerized application.

Model overview of Kubernetes
  Cluster
      -> Node
          -> Pods
              -> Containers 

Kubernetes runs clusters inside clusters there are multiple nodes and inside every node has pods and pods used to contain containers.


