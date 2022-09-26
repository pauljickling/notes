# Kubernetes

Kubernetes is an open-source container orchestration framework. It makes it easier to scale your applications by relying on a micro-services architecture, and provide high availability and recovery. The tradeoff, as typical from moves going from monolithic to micro-service architecture is greater upfront complexity.

## Architecture

Kubernetes deploys a master node that is an API server that all the other worker nodes rely on. It keeps track of the state of your inventory, schedules changes, a virtual network, and runs the etcd database which is a key-value db that maintains the state of the network.

The worker nodes run *pods*. Pods are a wrapper around a container or containers. They have their own IP address, and therefore acts as a self-contained server. Pods communicate with each other through their internal IP addresses.

Every worker node has three processes running:

1. The container runtime such as docker or podman
2. Kubelet which is the process that interacts with the container runtime and the node
3. The Kube Proxy which allows nodes to communicate with each other

In addition to pods, a node can also run what is called a *service*. Services provide a fixed IP address as opposed to pods that tend to have more dynamically changing IP addresses (whenever a pod dies and is restarted, which is frequent behavior in Kubernetes, a new IP address is generated). Services thus can also act as a load balancer for your application(s).

## Kube Flavors

- kubeadm: official Kubernetes tool, but heavyweight
- k3s: lightweight alternative provided by Rancher. Can run on Rasberry Pi.
- kind: kubernetes-in-docker
- minikube: tool for testing kubernetes locally
