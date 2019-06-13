# Docker Compose YAML

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
