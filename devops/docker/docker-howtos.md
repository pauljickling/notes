## Deploy a container
`docker run -d <foo/bar:x.y.z>`

`-d` is detached mode, which is almost always what you want so you can perform other tasks in the terminal without opening up an additional tab/window. `foo/bar` is the docker image repository, in some cases it can be shortened to just the name of the image if you have it locally. `:x.y.z` is optional and can specify a version number. You can also use tag names in place, `latest` being the most common.

## Get PID of container
`docker top <container name/id>`

You will be looking for the "master process".

## Get IP address of container
`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container name/id>`
