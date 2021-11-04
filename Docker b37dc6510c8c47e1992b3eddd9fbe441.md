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

## Docker prune

The `docker image prune` command allows you to clean up unused images. By default, the `docker image prune` only cleans up *dangling* images. A dangling image is one that is not tagged and is not referenced by any container. To remove dangling images:

```bash
docker image prune
```

To remove all images which are not used by existing containers, use the `-a` flag:

```bash
docker image prune -a
```

## Docker build

The `docker build` command builds Docker images from a `Dockerfile` and a “context”. A build’s context is the set of files located in the specified `PATH` or `URL`. The build process can refer to any of the files in the context. For example, your build can use a [copy](https://www.notion.so/ttps-docs-docker-com-engine-reference-builder-copy-7bf99b2be9044648948bdfd43b561600) instruction to reference a file in the context.

The `URL` parameter can refer to three kinds of resources: Git repositories, pre-packaged tarball contexts, and plain text files.

```bash
docker build [OPTIONS] PATH | URL | -
```

You can use `-t` to attach a tag to the image.

## Create an image from a container

You can start from a base image and apply your changes. Then you can create the new image:

```bash
# docker commit <container-name> <tag>
docker commit base_image my_new_image
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