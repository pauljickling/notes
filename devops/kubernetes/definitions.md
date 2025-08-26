# Definitions

## Context

A context is like a profile that defines the cluster being used, credentials for the cluster, and the definition of the namespace.

### Relevant Context Commands

`kubectl config current-context` Lists current context

`kubectl config get-contexts` Lists all contexts

`kubectl config use-context <context>` Switch contexts

## Cluster

A cluster is the kubernetes API server and control plane that manages the pods of the cluster.

### Relevant Cluster Commands

`kubectl cluster-info` Gets info on cluster

## Namespaces

Namespaces provide isolation for cluster resources. There is a defaulty namespace for every cluster, but it is best practice to always provide a defined namespace.

### Relevant Namespace Commands

`kubectl get namespaces` Lists all namespaces

`kubectl create namespace <namespace>` Create new namespace

`kubectl config set-context --current --<namespace>` Sets current context to specified namespace

`kubectl get pods -n <namespace>` Gets pods for the defined namespace. Works with any other `kubectl get <resource>` command. Use `-A` for all namespaces.

## Pods

Pods are a Kubernetes wrapper for a container.

### Relevant Pod Commands

`kubectl get pods`
