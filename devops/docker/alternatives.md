# Alternatives to Docker

Some open-source alternatives to docker to consider include podman, and nerdctl.

Podman is a Linux-only tool for creating and maintaining containers. Perhaps the most interesting aspect of Podman is that it is daemonless, so it doesn't need root privileges to operate.

nerdctl has several advanced features such as lazy loading of containers that allows them to run prior to completion of pulling, and running containers from encrypted images.

Both podman and nerdctl have docker compatible command line arguments, so you don't need to memorize a different set of commands to manage your containers. Any arguments you would make for docker will work with both tools.
