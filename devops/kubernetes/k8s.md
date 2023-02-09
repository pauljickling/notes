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

`kubectl create namespace {namespace}` create a namespace for your cluster. Before you create a namespace there is just a default namespace. To declare a namespace when using kubectl use the `-n` flag.

`watch kubectl get deployment {app name}` view deployment info

`kubectl describe pod -l {app name}` get pod information

`kubectl expose deployment {app name} --type=NodePort --port=80 --target-port=8080` deploys a web app exposing the http port using the kubernetes 8080 port. The `--type-NodePort` flag creates a node port service that takes traffic and routes it to pods

`kubectl get service {app name}` lists services running for an app

`kubectl get svc {app name}` shorthand for service

`kubectl edit deployment {app name}` edits the configuration file for an app

`kubectl set image deployment/{container name} {app name}={image path}` specifies an image to use for your application

`kubectl rollout history deployment {app name}` view revisions of app rollouts

`kubectl rollout undo deployment {app name}` rolls back to previous deployment

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

Rollouts can be managed via the `kubectl rollout` command.

## Helm

Helm is the package manager for Kubernetes. For browsing helm charts you can go to [https://artifacthub.io/](https://artifacthub.io/). Releases in Helm are instances of a helm configuration.

The manifest.yml is a representation of Kubernetes resources generated from a release chart. In the manifest you can define things like services, deployments, persistent storage, replicas, metadata, and namespaces, among other things. Namespaces in Kubernetes allow otherwise isolated containers and pods to communicate with one another, but not an externally facing network.

`helm install -f <path/to/yaml_config.yml> <name> <chart repo>` will install a chart for use with your cluster and include some default configs.

## Scaling

One common problem you need to deal with when configuring a Kubernetes cluster to be able to scale up as needed is dealing with stateful volumes. Most generic cloud volume resources can only attach to one pod at a time.

One solution might be to change how your application stores and retrieves data. For example, instead of retrieving data from a disk mounted volume, you might fetch data from some type of blob storage.

There are also solutions like [rook](rook.io) which is a management tool for file, block, and object storage, and is a good option for production grade environments, but does introduce additional complexity to your application architecture.

Some sort of FS client provisioner is another solution in use cases where you don't have so many pods running where it creates folders for any application that needs to write data, and any pod can then retrieve that data using a ReadWriteMany volume approach.

For horizontal pod auto-scaling you can use kubectl or edit configs. To start you'll need to use some sort of metrics server for monitoring load. You can check to see if the Metrics API is available with `kubectl top nodes`. There are several good options available such as Bitnami or Prometheus for exposing the Metrics API. Other metrics you might want to look at at a glance are `kubectl top pods -n <namespace>`.

To autoscale via CLI you can run `kubectl autoscale -n <namespace> deployment <deployment name> --min=n --max=m --cup-percent=l`. where `n`, `m`, and `l` are numeric values.

You can check information about horizontal autoscaling with `kubectl get hpa -n <namespace> <reference name>`.

Vertical pod auto-scaling is also possible, although you should be very careful with this as it can increase costs dramatically depending on your configuration.
