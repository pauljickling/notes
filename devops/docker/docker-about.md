# About Docker

Docker uses namespaces and cgroups tech to create an isolated workspace. This is what *containers* are. Anytime you use Docker to run a container it creates a set of *namespaces* for that container. It uses the following namespaces on Linux to create isolation for the container (note this list is non-exhaustive):

- The `pid` namespace: Process isolation
- The `net` namespace: Manages network interfaces
- The `ipc` namespace: Managing access to IPC resources
- The `mnt` namespace: Managing filesystem mount points
- The `uts` namespace: Isolating kernel and version identifiers

## Creating a New Container

A collection of files and instructions is what Docker calls an *image*.
