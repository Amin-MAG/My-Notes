# Docker

To see the docker system storage, you can use this:

```bash
docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          6         4         3.853GB   2.056GB (53%)
Containers      7         2         2.113GB   2.113GB (99%)
Local Volumes   9         4         33.97GB   21.82GB (64%)
Build Cache     30        0         96.28MB   96.28MB
```

## ENTRYPOINT VS CMD

You can use `ENTRYPOINT` or `CMD` in your Dockerfile to execute a command when the container starts. The difference between them is that `ENTRYPOINT` is executed any time the container is started, while `CMD` might be replaced with an command line option. Assuming the command you want to execute is `X`

```
docker run my-image Y

```

will execute `X` if `ENTRYPOINT X` was in the Dockerfile and `Y` if `CMD X` was in the Dockerfile.

Docker prune

The `docker image prune` command allows you to clean up unused images. By default, the `docker image prune` only cleans up *dangling* images. A dangling image is one that is not tagged and is not referenced by any container. To remove dangling images:

```bash
docker image prune
```

To remove all images which are not used by existing containers, use the `-a` flag:

```bash
docker image prune -a
```

## Add 

The **`ADD` command** is used to copy files/directories into a Docker image. It can copy data in _three_ ways:

-	Copy files from the local storage to a destination in the Docker image.
-   Copy a _tarball_ from the local storage and extract it automatically inside a destination in the Docker image.
-   Copy files from a URL to a destination inside the Docker image.

```dockerfile
ADD source destination
```

## Docker build

The `docker build` command builds Docker images from a `Dockerfile` and a “context”. A build’s context is the set of files located in the specified `PATH` or `URL`. The build process can refer to any of the files in the context. For example, your build can use a [copy](https://www.notion.so/ttps-docs-docker-com-engine-reference-builder-copy-7bf99b2be9044648948bdfd43b561600) instruction to reference a file in the context.

The `URL` parameter can refer to three kinds of resources: Git repositories, pre-packaged tarball contexts, and plain text files.

```bash
docker build [OPTIONS] PATH | URL | -
```

You can use `-t` to attach a tag to the image.

## Copy files

```bash
docker cp <CONTAINER_NAME>:<SRC_PATH> <DEST_PATH>
```

## Create an image from a container

You can start from a base image and apply your changes. Then you can create the new image:

```bash
# docker commit <container-name> <new_image_name>:<tag>
# the tag part is optional
docker commit base_image my_new_image:1.0.0
```

## Save

Used when you want to load a docker image. You can save docker image on disk by:

```bash
docker save busybox > busybox.tar
docker save myimage:latest | gzip > myimage_latest.tar.gz
```

## Load

To load a docker image from the disk.

```bash
docker load < busybox.tar.gz
docker load --input fedora.tar
```

## Start Container

```go
docker start <CONTAINER_NAME>
```

If you want to execute a command while you are starting the container you can use

```go
docker container exec -it kali_base2 /bin/bash
```

## Mount

To mount to your local file system

```bash
docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  nginx:latest
```

## Volumes

To create new volume

```bash
docker volume create hello
```

## Network

To see all of the networks:

```bash
sudo docker network ls
```

> The driver is actually the network type.

You can see more details of each network with `inspect`

```bash
sudo docker network inspect <network-name>
```

### Bridge Network

It's the default network for containers. You will need to map ports to communicate with these containers.

### User-Defined Bridge Network

It's better to define your network and that helps you to have isolation.

```bash
sudo docker network create asgard
```

### Host network

It shares everything like IP address with host. You don't need to map the ports.

### MAC VLan (bridge mode)

They will be connected directly to the home network and act like a separate system.

```bash
sudo docker network create -d macvaln --subnet 10.7.1.0/24 --gateway 10.7.1.3 -o parent=enp0s3 newasgard
```

### MAC VLan (802)

```bash
sudo docker network create -d macvaln --subnet 192.168.0.0/24 --gateway 192.168.20.1 -o parent=enp0s3.20 newasgard
```

### IP Vlan (l2)

The networks shares the MAC Address to solve the MAC Vlan issue.

```bash
sudo docker network create -d ipvlan --subnet 10.7.1.0/24 --gateway 10.7.1.3 -o parent=enp0s3 newasgard
```

### IP Vlan (l3)

```bash
sudo docker network create -d ipvlan --subnet 192.168.0.0/24 -o parent=enp0s3.20 -o ipvlan_mode=l3 --subnet 192.168.0.0/24 newasgard
```

### None

There is no network adapter here!

## Image Layers

You can see the image layers of an image using

```bash
docker history <IMAGE>:<TAG>
```

## Multi-Stage Builds

```docker
# Build stage
FROM maven AS build
WORKDIR /app
COPY myapp /app
RUN mvn package

# New stage by using FROM instruction
# Run stage
FROM tomcat 
# You can selectively copy artifacts from one stage to another
COPY --from=build /app/target/file.war /usr/local/tomcat/war/
```

## Scan

To scan your image for security vulnerabilities

```bash
# You need to login into docker hub
docker login
# Use synk
docker scan <IMAGE>:<TAG>
```

## Stats

To monitor the usage of the container you can use this command.

```bash
sudo docker stats <CONTAINER_NAME>
```

# Docker compose

## Run a container

You can jump into a container and start it with a specific command:

```bash
# sudo docker-compose run <service-name> <app-name>
sudo docker-compose run ubuntu bash
```

## Volumes

A single docker-compose service with a volume looks like this:

```yaml
version: "3.9"
services:
  frontend:
    image: node:lts
    volumes:
      - myapp:/home/node/app
volumes:
  myapp:
```

On the first invocation of `docker-compose up` the volume will be created. The same volume will be reused on the following invocations.

A volume might be created directly outside of composing with `docker volume create` and then referenced inside `docker-compose.yml` as follows:

```yaml
version: "3.9"
services:
  frontend:
    image: node:lts
    volumes:
      - myapp:/home/node/app
volumes:
  myapp:
    external: true
```

## Environment variables

You could use an external file for environment variables.

```yaml
version: "3.9"
services:
	env_file:
      - .env.compose
  frontend:
    image: node:lts
    volumes:
      - myapp:/home/node/app
volumes:
  myapp:
    external: true
```

For example here is the `.env.compose`

```
# Database
POSTGRES_DB=cobbler_db
POSTGRES_USER=admin
POSTGRES_PASSWORD=admin

# For development
# PgAdmin
PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=qweqwe
```

Or you can create a `environment` in your docker compose file.

```yaml
db:
    env_file:
      - .env.compose
    image: postgis/postgis:13-3.1
    container_name: my_db
    environment:
      # Extensions
      POSTGRES_MULTIPLE_EXTENSIONS: postgis,hstore,postgis_topology,postgis_raster,pgrouting
    ports:
      - "5432:5432"
```

# Best practices

1. User official docker images as base image: For example if you want to create docker file for you node app do not use ubuntu base image, use the node image.
2. Do not use latest tag (set more specific versions): Latest tags are unpredictable, you might get different docker image versions, and it might break stuff.
3. Don't use full-blown images: Because they are larger in image size, so they will take more storage space and needs more time for pushing and pulling too. There are lots of unnecessary tools in these images. They can add lots of vulnerabilities to your image. So you may introduce unnecessary security issues from the beginning.
4. Optimize caching image layers (You should order you commands in docker file from least to most frequently changing to take advantage of caching): Each docker file is consisted of image layers. Each step add a layer. Caching layers is important because when we are building application it will be much faster, even in pushing and pulling images only the non-cached layers will be pushed or pulled.
5. Use `.dockerignore` to reduce the image size (and maybe prevent unintended secret exposure)
6. User Multi-Stage builds (To separate build and runtime stage): That would result in increased image file and attack surface. (like `package.json` or `pom.xml`, JDK, Gradle, Maven)
7. Use the least privileged user: Default user for starting application in user `Id=0` or root. It is a security bad practice, because it will access to the docker host. 
8. Scan your image for vulnerabilities (Using tools like docker scan)

> This is from [https://www.youtube.com/watch?v=8vXoMqWgbQQ](https://www.youtube.com/watch?v=8vXoMqWgbQQ)

# Open Container Initiative

OCI standardized container runtime, image and distribution specifications. This ensures that container ecosystem remains open and not tied  to a single vendor.

Docker has popularized several key concepts

1. Standard Image
2. Streamline the Building of Container Images
3. Enabling Sharing of Images with Registeries
4. Facilitate the actual running of containers

# References

- https://www.youtube.com/watch?v=8vXoMqWgbQQ
- [Is Docker Still Relevant?](https://www.youtube.com/watch?v=Cs2j-Rjqg94
