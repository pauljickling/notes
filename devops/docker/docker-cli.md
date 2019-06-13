# Docker CLI

`docker login` Log in with your Docker ID, which you need to do to use a lot of Docker features

`docker run {container}` Runs docker container

`docker image ls` List docker images

`docker ps` or `docker container ls` Lists docker containers

`docker container create` Creates a docker container

`docker container stop {container}` Stops docker container

`docker build --tag={tag} .` Creates a docker image out of your Dockerfile with a name specified by the tag flag

`docker run -p 4000:80 {tag}` Runs the app where it maps the machine's port 4000 to the container's published port 80. You can now visit the app on your browser at localhost:4000. To run the app in the background in detached mode add a `-d` flag.

`docker tag image {username}/{repository}:{tag}` Associates a local imager with a repository on a registry

`docker push {username}/{repository}:{tag}` Uploads the tagged image to a repository

`docker swarm init` Before `docker stack deploy` can be used this needs to run

`docker stack deploy -c docker-compose.yml {app}` Runs docker stack and gives an app a name

`docker service ls` List of docker services

`docker stack rm {app}` Take the app down

`docker swarm leave --force` Take down the swarm
