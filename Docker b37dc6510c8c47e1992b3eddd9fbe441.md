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
