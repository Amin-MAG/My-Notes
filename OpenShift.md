
# OpenShift

## Resource

CPU is measured in units called milli cores. Each node in the cluster introspects the operating system to determine the amount of CPU cores on the node and then multiples that value by 1000 to express its total capacity. For example, if a node has 2 cores, the node’s CPU capacity would be represented as 2000m. If you wanted to use a 1/10 of a single core, you would represent that as 100m.

> In addition, it may be used with SI suffices (E, P, T, G, M, K, m) or their power-of-two-equivalents (Ei, Pi, Ti, Gi, Mi, Ki).
> 

### CPU Request

The CPU request represents a minimum amount of CPU that your container may consume, but if there is no contention for CPU, it may burst to use as much CPU as is available on the node. If there is CPU contention on the node, CPU requests provide a relative weight across all containers on the system for how much CPU time the container may use. On the node, CPU requests map to Kernel CFS shares to enforce this behavior.

### CPU Limit

CPU limits are used to control the maximum amount of CPU that your container may use independent of contention on the node. If a container attempts to use more than the specified limit, the system will throttle the container. This allows your container to have a consistent level of service independent of the number of pods scheduled to the node.

### Memory Request

In order to improve placement of pods in the cluster, it is recommended to specify the amount of memory required for a container to run. The scheduler will then take available node memory capacity into account prior to binding your pod to a node. A container is still able to consume as much memory on the node as possible even when specifying a request.

### Memory Limit

If you specify a memory limit, you can constrain the amount of memory your container can use. For example, if you specify a limit of 200Mi, your container will be limited to using that amount of memory on the node, and if it exceeds the specified memory limit, it will be terminated and potentially restarted dependent upon the container restart policy.

In the template

```yaml
spec:
  containers:
  - image: nginx
    name: nginx
    resources:
      requests:
        cpu: 100m 
        memory: 200Mi 
      limits:
        cpu: 200m 
        memory: 400Mi
```

More on [Quality of service tiers](https://docs.openshift.com/enterprise/3.1/dev_guide/compute_resources.html)

## Object Types

The CLI supports the following object types, some of which have abbreviated syntax

- build
- buildConfig - `bc`
- deploymentConfig - `dc`
- imageStream - `is`
- imageStreamTag - `istag`
- imageStreamImage - `isimage`
- event - `ev`
- node
- pod - `po`
- replicationController - `rc`
- service - `svc`
- persistentVolume - `pv`
- persistentVolumeClaim - `pvc`

# OC Basic Commands

## Who am I

```bash
# To get logged in user
oc whoami
```

## Get

```bash
# Get list of services
oc get svc
# You can filter based on selectors
oc get svc --selector=postgres-operator.crunchydata.com/cluster=hippo

oc get <object_type> [<object_name_or_id>]
```

## Describe

```bash
# Get details about the services
oc describe svc cobbler

oc describe <object_type> <object_id>
```

## Login

```bash
# OKD4
oc login --token=<TOKEN> --server=<SERVER>
```

## Process

To change the templates `.yml` files to config files, you need to `process` the templates.

```bash
oc process -f <TEMPLATE_FILE> > oc.build.config
```

At the end a new file called `os.build.config` will be created.

We can pass some parameters based on the templates with `--params`.

```bash
oc process -n <NAMESPACE> -f <TEMPLATE_DIR_PATH>/template.yml \
     --param APP_NAME=hello-world \
     --param NAMESPACE=apis \
     --param REGION=teh-1 \
	   > oc.build.config
```

## Apply

To send the configuration file to the cluster to be configured, use:

```bash
oc apply -f <CONFIG_FILE_PATH>
```

You can use the pipe concept to connect the process and apply the command:

```bash
oc process -n <NAMESPACE> -f <TEMPLATE_DIR_PATH>/template.yml \
     --param APP_NAME=monshi-staging \
     --param NAMESPACE=smapp-api \
     --param REGION=teh-1 | oc apply -f -
```

## Start-Build

To start a new build based on the provided build config.

```bash
oc start-build <BUILD_CONFIG>
```

- You can use the `-F` or `--follow` flag to watch its logs until it completes or fails.
- Use `-w` or `--wait` to wait for build and exit with a non-zero return code if the build fails.

```bash
oc start-build -F hello-world
```

## Rollout

To start a new deployment based on the latest.

```bash
oc rollout latest dc/<name>
```

## Tag

```bash
# To delete specific image from image stream
oc tag -d <IMAGE_STREAM_NAME>:<TAG>
```

## History

To get basic information about all the available revisions of your application:

```bash
oc rollout history dc/<name>
```

This will show details about all recently created replication controllers for the provided deployment configuration, including any currently running deployment process.

You can view details specific to a revision by using the `--revision` flag:

```bash
oc rollout history dc/<name> --revision=1
```

## Logs

```bash
oc logs
```

## SSH to pod

```bash
oc rsh <pod>
```

## Execute a command 

```bash
oc exec -it cobbler-staging-106-r7bx4 -- cat /tmp/data/data-sources/stalin-web/feedbacks_ret.csv
```

## Quota

```bash
 # To get the quotas in the namespace
oc get quota -n smapp-data

# To see the details you can use `describe` as usual
oc describe quota default -n smapp-data

# To see the limits
oc describe limits default
```

## User Tokens

```bash
oc get useroauthaccesstokens
```

# Objects

## Config Map

### Create config map

To create a config map using the `oc` commands:

```bash
oc create configmap <configmap_name> [options]
```

You can use the following command to create a **`ConfigMap`** holding the content of each file in this directory:

```bash
oc create configmap game-config --from-file=example-files/
```

You can specify a single file or even a single variable using `--from-literal=key1.key2=value`. If you want to create a config map using a template, you can use:

```bash
oc create -f <TEMPLATE_PATH>
```

Here is a sample template for the config map:

```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: hello-world-config
  labels:
    app: hello-world
data:
  variables: |
    global:
      readTimeout: 2m
      readHeaderTimeout: 2m
      writeTimeout: 2m
      idleTimeout: 2m
      maxHeaderBytes: 8196
      gatewayPort: 8080
      adminPort: 8081
      metricsPort: 8082
  default_admins: |
    {
      "admins": [
        {
          "name": "Roth Workman",
          "gender": "male",
          "company": "ACCUFARM",
          "email": "rothworkman@accufarm.com",
          "phone": "+1 (981) 417-3526"
        },
        {
          "name": "Randi Martinez",
          "gender": "female",
          "company": "APPLICA",
          "email": "randimartinez@applica.com",
          "phone": "+1 (895) 466-2642"
        },
        {
          "name": "Enid Molina",
          "gender": "female",
          "company": "ZOLAREX",
          "email": "enidmolina@zolarex.com",
          "phone": "+1 (874) 595-2467"
        }
      ]
    }
```

### Get the values

To see the configs you can use the `oc describe`:

```bash
oc describe configmaps <CONFIG_NAME>
```

You can use `oc get` too:

```bash
oc get configmaps hello-world-config-1 -o yaml
```

### Consuming in environment variables

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: gcr.io/google_containers/busybox
      command: [ "/bin/sh", "-c", "env" ]
      env: 
        - name: SPECIAL_LEVEL_KEY
          valueFrom:
            configMapKeyRef:
              name: special-config 
              key: special.how 
        - name: SPECIAL_TYPE_KEY
          valueFrom:
            configMapKeyRef:
              name: special-config 
              key: special.type 
              optional: true 
      envFrom: 
        - configMapRef:
            name: env-config 
  restartPolicy: Never
```

 

- Stanza to pull the specified environment variables from a ConfigMap.
- Name of the ConfigMap to pull specific environment variables from.
- Environment variable to pull from the ConfigMap.
- Makes the environment variable optional. As optional, the pod will be started even if the specified ConfigMap and keys do not exist.
- Stanza to pull all environment variables from a ConfigMap.
- Name of the ConfigMap to pull all environment variables.

### Setting Command-line Arguments

You can use `$(VARIABLE_NAME)` to get the value.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: gcr.io/google_containers/busybox
      command: [ "/bin/sh", "-c", "echo $(SPECIAL_LEVEL_KEY) $(SPECIAL_TYPE_KEY)" ]
      env:
        - name: SPECIAL_LEVEL_KEY
          valueFrom:
            configMapKeyRef:
              name: special-config
              key: special.how
        - name: SPECIAL_TYPE_KEY
          valueFrom:
            configMapKeyRef:
              name: special-config
              key: special.type
  restartPolicy: Never
```

### Consuming in volumes

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: gcr.io/google_containers/busybox
      command: [ "/bin/sh", "cat", "/etc/config/special.how" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: special-config
  restartPolicy: Never
```

You can also control the paths within the volume where ConfigMap keys are projected:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: gcr.io/google_containers/busybox
      command: [ "/bin/sh", "cat", "/etc/config/path/to/special-key" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: special-config
        items:
        - key: special.how
          path: path/to/special-key
  restartPolicy: Never
```

# Snippets

## MinIO

```yaml
apiVersion: v1
kind: Template
labels:
  app: minio
  template: minio
message: |-
  The following service(s) have been created in your project: ${NAME}.

  For more information about using this template, including OpenShift considerations, see https://github.com/elmanytas/minio-openshift/blob/master/README.md.
metadata:
  annotations:
    description: |-
      A min.io service.  For more information about using this template, including OpenShift considerations, see https://github.com/elmanytas/minio-openshift/blob/master/README.md.

      WARNING: This template needs a default storage class with space enough.
    iconClass: icon-database
    openshift.io/display-name: min.io
    openshift.io/documentation-url: https://github.com/elmanytas/minio-openshift
    openshift.io/long-description: This template defines resources needed to deploy
      a min.io service that allows you to use a AWS S3 compatible api in your apps.
    tags: minio,min.io,s3
    template.openshift.io/bindable: "false"
  name: minio
objects:
- apiVersion: v1
  kind: Secret
  metadata:
    name: ${NAME}-keys
  stringData:
    access-key: ${MINIO_ACCESS_KEY}
    secret-key: ${MINIO_SECRET_KEY}
- apiVersion: v1
  kind: Service
  metadata:
    annotations:
      description: Exposes the application pod
      service.alpha.openshift.io/dependencies: '[{"name": "${MINIO_SERVICE_NAME}", "kind": "Service"}]'
    name: ${NAME}
  spec:
    ports:
    - name: ${NAME}
      port: 9000
      targetPort: 9000
    selector:
      app: ${NAME}
- apiVersion: v1
  kind: Route
  metadata:
    name: ${NAME}
  spec:
    host: ${APPLICATION_DOMAIN}
    to:
      kind: Service
      name: ${NAME}
- apiVersion: apps/v1
  kind: StatefulSet
  metadata:
    labels:
      app: ${NAME}
    name: ${NAME}
  spec:
    podManagementPolicy: OrderedReady
    replicas: 1
    revisionHistoryLimit: 1
    selector:
      matchLabels:
        app: ${NAME}
    serviceName: ${NAME}
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: ${NAME}
      spec:
        containers:
        - args:
          - server
          - /data
          env:
          - name: MINIO_ACCESS_KEY
            valueFrom:
              secretKeyRef:
                key: access-key
                name: ${NAME}-keys
          - name: MINIO_SECRET_KEY
            valueFrom:
              secretKeyRef:
                key: secret-key
                name: ${NAME}-keys
          image: karrier/minio:RELEASE.2018-09-01T00-38-25Z
          imagePullPolicy: IfNotPresent
          name: ${NAME}
          ports:
          - containerPort: 9000
            protocol: TCP
          resources:
            limits:
              cpu: 200m
              memory: ${MEMORY_LIMIT}
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
          volumeMounts:
          - mountPath: /data
            name: data
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext: {}
        terminationGracePeriodSeconds: 30
    updateStrategy:
      type: OnDelete
    volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes:
        - ReadWriteOnce
        resources:
          requests:
            storage: 10Gi
parameters:
- description: The name assigned to all of the frontend objects defined in this template.
  displayName: Name
  name: NAME
  required: true
  value: minio
- description: Maximum amount of memory the container can use.
  displayName: Memory Limit
  name: MEMORY_LIMIT
  required: true
  value: 512Mi
- description: The exposed hostname that will route to the Min.io service, if left
    blank a value will be defaulted.
  displayName: Application Hostname
  name: APPLICATION_DOMAIN
- description: Access key for min.io api.
  displayName: Access Key
  from: '[a-zA-Z0-9]{32}'
  generate: expression
  name: MINIO_ACCESS_KEY
- description: Secret key for min.io api.
  displayName: Secret Key
  from: '[a-zA-Z0-9]{32}'
  generate: expression
  name: MINIO_SECRET_KEY
```

# Resources

[Basic Deployment Operations](https://docs.openshift.com/container-platform/3.3/dev_guide/deployments/basic_deployment_operations.html)