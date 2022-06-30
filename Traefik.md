# Traefik

For a quick start, you can create the reverse proxy container like below.

```yml
version: '3'
services:
  reverse-proxy:
    # The official v2 Traefik docker image
    image: traefik:v2.7
    container_name: traefik_demo
    # Enables the web UI and tells Traefik to listen to docker
    command: --api.insecure=true --providers.docker
    ports:
      # The HTTP port
      - "80:80"
      # The Web UI (enabled by --api.insecure=true)
      - "8089:8080"
    volumes:
      # So that Traefik can listen to the Docker events
      - /var/run/docker.sock:/var/run/docker.sock
```

> You Can see all raw details about the running containers by calling `http://188.121.110.10:8089/api/rawdata` API 

## Edge Router

Traefik is an *Edge Router*, it means that it's the door to your platform.

## Auto Service Discovery

Traefik has automatically detected the new container and updated its own configuration.

# Resources

- [Traefik Documentation](https://doc.traefik.io/)
