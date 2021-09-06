# Miscellaneous

# Init Container

In Kubernetes, an init container is **the one that starts and executes before other containers in the same Pod**. It's meant to perform initialization logic for the main application hosted on the Pod. For example, create the necessary user accounts, perform database migrations, create database schemas, and so on.

```yaml
initContainers:
            - name: init-tile38-data
              image: docker-registry.default.svc:5000/smapp-storage/aws-cli:${AWS_CLI_VERSION}
              command: ["sh","-c","aws s3 --endpoint-url=$ENDPOINT cp s3://$BUCKET/appendonly-$DATE.aof /data/appendonly.aof"]
              env:
                - name: ENDPOINT
                  value: ${S3_ENDPOINT}
                - name: BUCKET
                  value: ${S3_BUCKET}
                - name: DATE
                  value: ${AOF_DATE}
                - name: AWS_ACCESS_KEY_ID
                  valueFrom:
                    secretKeyRef:
                      key: accesskey
                      name: ${MINIO_SECRET}
                - name: AWS_SECRET_ACCESS_KEY
                  valueFrom:
                    secretKeyRef:
                      key: secretkey
                      name: ${MINIO_SECRET}
              volumeMounts:
                - mountPath: "/.aws/config"
                  name: ${APP_NAME}-aws-cli-conf
                  subPath: config
                - mountPath: /data
                  name: ${APP_NAME}-data
                - mountPath: /root/.aws
                  name: aws
                - mountPath: /project
                  name: project
              resources:
                limits:
                  memory: 1000Mi
```

# References

[Kubernetes Patterns : The Init Container Pattern](https://www.magalix.com/blog/kubernetes-patterns-the-init-container-pattern#)