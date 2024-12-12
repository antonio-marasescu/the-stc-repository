# Kubernetes

## Container

- What is a container
- How they are isolated
  - Containers vs Virtual Machines
- Images
- How to run a container

## Kubernetes Concepts

Describe purpose to scale up/down based on load, load balancing and how it helps on orchestrate this.

### Components of Kubernetes

- API Server, etcd, kubelet, container runtime, controller, scheduler

### Nodes and Cluster

- What is a Node
- What is a Cluster
- What is a Master Node
  - kube-apiserver
  - etcd
  - controller
  - scheduler
- What is a Worker Node
  - kubelet

    
### Pods

- What is a Pod: The smallest object you can create in kubernetes, it runs in a node.
- What are Multi-container Pods
  - How to scale in a pod

## Command Line Utilities

### Concepts
- Containerd
- ctr
- nerdctl
- crictl
- kubectl

### Useful Commands

- Create a NGINX Pod
    ```shell
    kubectl run nginx --image=nginx
    ```
- Create a deployment using imperative command
    ```shell
    kubectl create deployment nginx --image=nginx
    ```