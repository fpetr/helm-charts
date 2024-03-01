# Readium License Server

## Parameters

### Image configuration

| Name               | Description                                      | Value                                                                    |
| ------------------ | ------------------------------------------------ | ------------------------------------------------------------------------ |
| `image.repository` | Image repository                                 | `registry.gitlab.com/skczu/elektronicke-skripta/readium-stack/lcpserver` |
| `image.pullPolicy` | Image pull policy                                | `IfNotPresent`                                                           |
| `image.tag`        | Image tag                                        | `latest`                                                                 |
| `imagePullSecrets` | Specify docker-registry secret names as an array | `[]`                                                                     |

### License server parameters

| Name                            | Description                                                                                                                                                             | Value                                       |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| `profile`                       | Value of LCP profile. Allowed values are "basic" or "1.0"                                                                                                               | `basic`                                     |
| `configDirectory`               | Application directory for mounting configuration files and certificate                                                                                                  | `/app`                                      |
| `auth.username`                 | Username for API requests authentication                                                                                                                                | `readium`                                   |
| `auth.password`                 | Password for API requests authentication                                                                                                                                | `readium`                                   |
| `auth.existingSecret`           | Name of existing secret containing key "htpasswd" with username and password encrypted using htpasswd utility                                                           | `""`                                        |
| `auth.authfileMountDir`         | Name of directory where to mount htpasswd file                                                                                                                          | `/app/htpasswd`                             |
| `lcp.host`                      | Public server hostname                                                                                                                                                  | `"0.0.0.0"`                                 |
| `lcp.port`                      | Listening port. Default to service.port parameter                                                                                                                       | `""`                                        |
| `lcp.public_base_url`           | URL used by the Status Server and the Frontend Server to communicate with this License server. Automatically constructed using name of created service and service port | `""`                                        |
| `lcp.readonly`                  | Default to false                                                                                                                                                        | `""`                                        |
| `database.host`                 | MySQL database host. Default host is automatically constructed from mysql dependency included in this chart.                                                            | `""`                                        |
| `database.port`                 | MySQL database port.                                                                                                                                                    | `3306`                                      |
| `database.database`             | MySQL database name. Default to mysql.auth.database parameter                                                                                                           | `""`                                        |
| `database.username`             | MySQL username name. Default to mysql.auth.username parameter                                                                                                           | `""`                                        |
| `database.password`             | MySQL password name. Default to mysql.auth.password parameter                                                                                                           | `""`                                        |
| `certificate.mountPath`         | Base path for mounting certificates                                                                                                                                     | `/app/cert`                                 |
| `certificate.certificateBase64` | Base64 encoded certificate                                                                                                                                              | `""`                                        |
| `certificate.privatekeyBase64`  | Base64 encoded private key                                                                                                                                              | `""`                                        |
| `certificate.existingSecret`    | Name of existing secret containing certificate and private key. Secret must contain keys with name "cert.pem" and "key.pem"                                             | `""`                                        |
| `lsdConfig.public_base_url`     | Public base URL for LSD (Licensing Status Server)                                                                                                                       | `""`                                        |
| `lsdConfig.username`            | Username for LSD authentication                                                                                                                                         | `""`                                        |
| `lsdConfig.password`            | Password for LSD authentication                                                                                                                                         | `""`                                        |
| `objectStoreS3.endpoint`        | Endpoint URL for S3 object store                                                                                                                                        | `http://minio.minio.svc.cluster.local:9000` |
| `objectStoreS3.access_id`       | Access ID for S3 authentication                                                                                                                                         | `readium`                                   |
| `objectStoreS3.secret`          | Secret key for S3 authentication                                                                                                                                        | `""`                                        |
| `objectStoreS3.token`           | Token for S3 authentication (optional)                                                                                                                                  | `""`                                        |
| `objectStoreS3.bucket`          | Bucket name                                                                                                                                                             | `readium`                                   |
| `objectStoreS3.region`          | Region of the S3 bucket                                                                                                                                                 | `us-east-1`                                 |
| `objectStoreS3.disable_ssl`     | Disable SSL for S3 connection                                                                                                                                           | `true`                                      |
| `objectStoreS3.pathStyle`       | Use path-style addressing for S3 (optional)                                                                                                                             | `""`                                        |
| `localization.languages`        | Comma-separated array of supported languages                                                                                                                            | `[]`                                        |
| `localization.folder`           | Folder for localization files                                                                                                                                           | `""`                                        |
| `localization.default_language` | Default language for localization                                                                                                                                       | `""`                                        |
| `aes256_cbc_or_gcm`             | Description for aes256_cbc_or_gcm parameter                                                                                                                             | `_GCM`                                      |

### Common parameters

| Name                                         | Description                                                                                                            | Value       |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ----------- |
| `nameOverride`                               | String to partially override lcpserver.name template (will maintain the release name)                                  | `""`        |
| `fullnameOverride`                           | String to fully override lcpserver.name template                                                                       | `""`        |
| `replicaCount`                               | Desired number of replicas                                                                                             | `1`         |
| `serviceAccount.create`                      | Specifies whether a service account should be created                                                                  | `true`      |
| `serviceAccount.automount`                   | Automatically mount a ServiceAccount's API credentials                                                                 | `true`      |
| `serviceAccount.annotations`                 | Annotations to add to the service account                                                                              | `{}`        |
| `serviceAccount.name`                        | The name of the service account to use. If not set and create is true, a name is generated using the fullname template | `""`        |
| `podAnnotations`                             | Additional annotations to add to the pods                                                                              | `{}`        |
| `podLabels`                                  | Additional labels to add to the pods.                                                                                  | `{}`        |
| `podSecurityContext`                         | Security context for the pods.                                                                                         | `{}`        |
| `securityContext`                            | Security context for containers within the pods.                                                                       | `{}`        |
| `service.type`                               | Type of Kubernetes service (e.g., ClusterIP, NodePort, LoadBalancer)                                                   | `ClusterIP` |
| `service.port`                               | Port number for the service.                                                                                           | `8080`      |
| `ingress.enabled`                            | Enable or disable Ingress.                                                                                             | `false`     |
| `ingress.className`                          | Ingress class name.                                                                                                    | `""`        |
| `ingress.annotations`                        | Additional annotations for the Ingress.                                                                                | `{}`        |
| `ingress.hosts`                              | Hostnames associated with the Ingress.                                                                                 | `{}`        |
| `ingress.tls`                                | TLS configuration for the Ingress.                                                                                     | `[]`        |
| `resources`                                  | Resource requests and limits configuration                                                                             | `{}`        |
| `autoscaling.enabled`                        | Enable or disable Horizontal Pod Autoscaler.                                                                           | `false`     |
| `autoscaling.minReplicas`                    | Minimum number of replicas.                                                                                            | `1`         |
| `autoscaling.maxReplicas`                    | Maximum number of replicas.                                                                                            | `100`       |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization percentage.                                                                                     | `80`        |
| `additionalVolumes`                          | Array of additional volumes to create                                                                                  | `[]`        |
| `additionalVolumeMounts`                     | Array of additional volumes to mount to container                                                                      | `[]`        |
| `nodeSelector`                               | Node selector configuration                                                                                            | `{}`        |
| `tolerations`                                | Tolerations configuration                                                                                              | `[]`        |
| `affinity`                                   | Affinity configuration                                                                                                 | `{}`        |

### MySQL parameters

| Name                             | Description                                                          | Value       |
| -------------------------------- | -------------------------------------------------------------------- | ----------- |
| `mysql.enabled`                  | Enable MySQL                                                         | `true`      |
| `mysql.auth.rootPassword`        | Root password for MySQL                                              | `lcpserver` |
| `mysql.auth.database`            | Database name for MySQL                                              | `lcpserver` |
| `mysql.auth.username`            | Username for MySQL                                                   | `lcpserver` |
| `mysql.auth.password`            | Password for MySQL                                                   | `lcpserver` |
| `mysql.primary.persistence.size` | Size of persistent volume                                            | `1Gi`       |
| `mysql.initdbScripts`            | Initialization scripts containing required tables for License server | `{}`        |
