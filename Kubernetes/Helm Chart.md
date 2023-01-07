# What is Helm

It is a package manager for k8s. It like apt, home brew or yum. It is a good way to package YAML files together and distribute them in public and private repositories.

## Helm Chart

If you want to create a service like elastic stack for logging, you may need configuring and creating different kinds of components like `services`, `statefulset`, `secrets`, `configmaps`, and so on. Testing and running all of them is a pain in the neck.

What about once packaging all of this YAML files together with a common standard and then use it again and again? This packaged YAML files are called Helm Chart.

So you can bundle your YAML files and then Push it to some repositories.

You can search what you’re looking for by using `search` (You can use helm hub)

```bash
helm search <keyword>
```

### Structure

`Chart.yml` contains all of the meta data of the application.

`values.yml` sets all of the variables for template files.

`charts/` contains the chart dependencies

`templates/` contains the actual template files

To deploy YAML files to k8s cluster:

```bash
helm install <chart-name>

# You can specify the value file or --set for a single variable
helm install --values=<path> <chart-name>
```

## Template Engine

Helm next feature is being a template engine. Most of deploying micro services is pretty much the same. You can define a blue print for your micro services and create some dynamic values, then you don’t need to write YAML files for each one of them.

At last you need just a `values.yml` file (or `--set` command) to set the variables inside the configurations.

## Release Management

### Helm v2

In version 2 we have a helm client like helm CLI to execute the commands. After installing the helm chart it sends the request to the Tiller to apply changes to k8s cluster. (Tiller is the server-side of helm.)

Tiller actually saves the history of the deployed YAML files for future.

### Helm v3

This feature was removed because of security issues. It was powerful and could add, update, or remove components of k8s cluster. There is no Tiller.

# Commands

To add a new repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

To list the repositories

```bash
helm repo list
```

To search between the repositories

```bash
helm search repo bitnami/nginx
```

To install a helm chart with a parameter

```bash
helm install cobbler-db-postgresql bitnami/postgresql --set service.type=CluterIP

# You can also uninstall a release using 
helm uninstall cobbler-db-postgresql
```

You can see the list of the helm's deployments

```bash
helm ls
```

Create a new helm chart

```bash
helm create cobbler-pg-database
```

To clean up and see whether you make a mistake or not

```bash
helm lint ./mychart/
```


# References

[What is Helm in Kubernetes? Helm and Helm Charts explained | Kubernetes Tutorial 23](https://www.youtube.com/watch?v=-ykwb1d0DXU)