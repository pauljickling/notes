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

There are lots of tools for setting up a Kubernetes environment. Here are a few:

- kubeadm: official Kubernetes tool, but heavyweight
- k3s: lightweight alternative provided by Rancher. Can run on Rasberry Pi.
- kind: kubernetes-in-docker
- minikube: tool for testing kubernetes locally

Additionally, most cloud providers offer a managed Kubernetes service which takes the pain out of having to configure your own master node.

## CLI

`kubectl` is the command line interface for managing your Kubernetes infrastrucutre. It does not deploy a cluster, but rather is used to manage defined infrastructure.

`kubectl get nodes` will list deployed infrastructure

`kubectl create deployment {app name} --image={container hub/image:version}` deploys app with docker image

`watch kubectl get deployment {app name}` view deployment info

`kubectl describe pod -l {app name}` get pod information

`kubectl expose deployment {app name} --type=NodePort --port=80 --target-port=8080` deploys a web app exposing the http port using the kubernetes 8080 port. The `--type-NodePort` flag creates a node port service that takes traffic and routes it to pods

`kubectl get service {app name}` lists services running for an app

`kubectl get svc {app name}` shorthand for service

`kubectl edit deployment {app name}` edits the configuration file for an app

`kubectl set image deployment/{container name} {app name}={image path}` specifies an image to use for your application

`kubectl delete pod {pod name}` deletes a pod

`kubectl create secret {secret type}` creates a secret when kubernetes needs sensitive data like passwords, API tokens, etc.

`kubectl get secrets` view available secrets

`kubectl logs -f -l app={app name}` view app logs. `-l` flag adds labels which is useful when you are running multiple pods.

## Kubeconfig

This is a yml config file that describes how to connect to a kubernetes control plane. Once you have your defined config you will need to have it exposed to kubectl, which you can do in bash with `export KUBECONFIG=/path/to/config` (the path is probably in $HOME/.kube)

## Deploying Apps

To create replicas of a service edit the kubeconfig file and change `replicas` under `spec`.

You can view logs for your apps 

When making changes to a config for a deployed app it is recommended to commit changes before changes are applied so if there is an issue you can roll back.
