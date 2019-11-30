# Docker Compose YAML

`docker-compose` is a command line interface that lets you build and deploy apps with the aid of a `docker-compose.yml` file.

The `docker-compose.yml` file defines how a Docker container behaves in production.

```
version: "{version no}"
services:
  web:
    image: {username}/{repo}:{tag}
    deploy:
      replicas: {replica no}
      resources:
        limits:
          cpus: "{cpu value}"
          memory: {memory value}
        restart_policy:
          condition: {condition}
      ports:
        - "{machine port}:{docker port}"
      networks:
        - webnet
    networks:
      webnet:
```

The docker compose file can define the sorts of flags you would need to remember and reuse in the shell to simplify deployment.
