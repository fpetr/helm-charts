# Readium Status Server Helm Chart
Install [readium-lsd-server](https://github.com/readium/readium-lcp-server) on kubernetes using mysql. Currently there is no official docker image available. You need to build your own or use one provided with this helm chart. There is already example in [official repository](https://github.com/readium/readium-lcp-server/tree/cd/docker).

Disclaimer: This chart is not suitable for production usage. It only contains test certificate and basic LCP profile. This profile, because it is open, does not offer any security. Security is provided by a "production" profile, i.e. confidential crypto information and a personal X.509 certificate delivered to trusted implementers by [EDRLab](mailto:contact@edrlab.org). EDRLab is the wordwide LCP Certification Authority. Licenses generated with the "production" profile are handled by any LCP compliant Reading System.

## Prerequisites
- Kubernetes 1.19+
- Helm 3+
- PV provisioner support in the underlying infrastructure

## Dependencies
By default this chart installs additional, dependent charts:
- [bitnami/mysql](https://github.com/bitnami/charts/tree/main/bitnami/mysql)

## Installing the Chart
Add chart repository:

```console
helm repo add fpetr https://fpetr.github.io/helm-charts
helm repo update
```

To install the chart with the release name `my-release`:

```console
helm install my-release fpetr/readium-lsdserver
```

These commands deploy Readium Status Server on the Kubernetes cluster in the default configuration using MySQL as database. The [Parameters](#parameters) section lists the parameters that can be configured during installation.


## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release. As MySQL dependency is Stateful set application, you need to manually remove created PVC.

## Parameters

### Image configuration

| Name               | Description                                                               | Value                                                    |
| ------------------ | ------------------------------------------------------------------------- | -------------------------------------------------------- |
| `image.repository` | Image repository. There is no official public image yet.                  | `ghcr.io/fpetr/readium-lcp-server-docker-helm/lsdserver` |
| `image.pullPolicy` | Image pull policy                                                         | `IfNotPresent`                                           |
| `image.tag`        | Image tag. Overrides the image tag whose default is the chart appVersion. | `""`                                                     |
| `imagePullSecrets` | Specify docker-registry secret names as an array                          | `[]`                                                     |

### Status server parameters

| Name                            | Description                                                                                                                                         | Value                                 |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| `configDirectory`               | Application directory for mounting configuration files and certificate                                                                              | `/app`                                |
| `auth.username`                 | Username for API requests authentication                                                                                                            | `readium`                             |
| `auth.password`                 | Password for API requests authentication                                                                                                            | `readium`                             |
| `auth.existingSecret`           | Name of existing secret containing key "htpasswd" with username and password encrypted using htpasswd utility                                       | `""`                                  |
| `auth.authfileMountDir`         | Name of directory where to mount htpasswd file                                                                                                      | `/app/htpasswd`                       |
| `lsd.host`                      | Public server hostname                                                                                                                              | `"0.0.0.0"`                           |
| `lsd.port`                      | Listening port. Default to service.port parameter                                                                                                   | `""`                                  |
| `lsd.public_base_url`           | The URL used by the License Server to communicate with this Status Server. Automatically constructed using name of created service and service port | `""`                                  |
| `lsd.readonly`                  | Default to false                                                                                                                                    | `""`                                  |
| `lsd.licence_link_url`          | Url from which a fresh license can be fetched from the provider's frontend server                                                                   | `""`                                  |
| `database.host`                 | MySQL database host. Default host is automatically constructed from mysql dependency included in this chart.                                        | `""`                                  |
| `database.port`                 | MySQL database port.                                                                                                                                | `3306`                                |
| `database.database`             | MySQL database name. Default to mysql.auth.database parameter                                                                                       | `""`                                  |
| `database.username`             | MySQL username name. Default to mysql.auth.username parameter                                                                                       | `""`                                  |
| `database.password`             | MySQL password name. Default to mysql.auth.password parameter                                                                                       | `""`                                  |
| `certificate.mountPath`         | Base path for mounting certificates                                                                                                                 | `/app/cert`                           |
| `certificate.certificateBase64` | Base64 encoded certificate. If empty, uses default test certificate                                                                                 | `""`                                  |
| `certificate.privatekeyBase64`  | Base64 encoded private key. If empty, uses default test private key                                                                                 | `""`                                  |
| `certificate.existingSecret`    | Name of existing secret containing certificate and private key. Secret must contain keys with name "cert.pem" and "key.pem"                         | `""`                                  |
| `lcpConfig.public_base_url`     | Public base URL for LCP (Licensing Server)                                                                                                          | `http://readium-lcpserver:8080`       |
| `lcpConfig.username`            | Username for LCP authentication                                                                                                                     | `""`                                  |
| `lcpConfig.password`            | Password for LCP authentication                                                                                                                     | `""`                                  |
| `objectStoreS3.enabled`         | Enable configuration of Licence Server object store                                                                                                 | `true`                                |
| `objectStoreS3.endpoint`        | Endpoint URL for S3 object store.                                                                                                                   | `http://readium-lcpserver-minio:9000` |
| `objectStoreS3.access_id`       | Access ID for S3 authentication                                                                                                                     | `readium`                             |
| `objectStoreS3.secret`          | Secret key for S3 authentication                                                                                                                    | `readiumreadium`                      |
| `objectStoreS3.token`           | Token for S3 authentication (optional)                                                                                                              | `""`                                  |
| `objectStoreS3.bucket`          | Bucket name                                                                                                                                         | `readium`                             |
| `objectStoreS3.region`          | Region of the S3 bucket                                                                                                                             | `us-east-1`                           |
| `objectStoreS3.disable_ssl`     | Disable SSL for S3 connection                                                                                                                       | `true`                                |
| `objectStoreS3.pathStyle`       | Use path-style addressing for S3 (optional)                                                                                                         | `""`                                  |
| `localization.languages`        | Comma-separated array of supported languages                                                                                                        | `[]`                                  |
| `localization.folder`           | Folder for localization files                                                                                                                       | `""`                                  |
| `localization.default_language` | Default language for localization                                                                                                                   | `""`                                  |

### Common parameters

| Name                                         | Description                                                                                                                  | Value           |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | --------------- |
| `nameOverride`                               | String to partially override lsdserver.name template (will maintain the release name)                                        | `""`            |
| `fullnameOverride`                           | String to fully override lsdserver.name template                                                                             | `""`            |
| `replicaCount`                               | Desired number of replicas                                                                                                   | `1`             |
| `strategy`                                   | Deployment strategy to use. Defaults to RollingUpdate. If you enabled fileSystemStore persistence, change this to "Recreate" | `RollingUpdate` |
| `serviceAccount.create`                      | Specifies whether a service account should be created                                                                        | `true`          |
| `serviceAccount.automount`                   | Automatically mount a ServiceAccount's API credentials                                                                       | `true`          |
| `serviceAccount.annotations`                 | Annotations to add to the service account                                                                                    | `{}`            |
| `serviceAccount.name`                        | The name of the service account to use. If not set and create is true, a name is generated using the fullname template       | `""`            |
| `podAnnotations`                             | Additional annotations to add to the pods                                                                                    | `{}`            |
| `podLabels`                                  | Additional labels to add to the pods.                                                                                        | `{}`            |
| `podSecurityContext`                         | Security context for the pods.                                                                                               | `{}`            |
| `securityContext`                            | Security context for containers within the pods.                                                                             | `{}`            |
| `service.type`                               | Type of Kubernetes service (e.g., ClusterIP, NodePort, LoadBalancer)                                                         | `ClusterIP`     |
| `service.port`                               | Port number for the service.                                                                                                 | `8080`          |
| `ingress.enabled`                            | Enable or disable Ingress.                                                                                                   | `false`         |
| `ingress.className`                          | Ingress class name.                                                                                                          | `""`            |
| `ingress.annotations`                        | Additional annotations for the Ingress.                                                                                      | `{}`            |
| `ingress.hosts`                              | Hostnames associated with the Ingress.                                                                                       | `{}`            |
| `ingress.tls`                                | TLS configuration for the Ingress.                                                                                           | `[]`            |
| `resources`                                  | Resource requests and limits configuration                                                                                   | `{}`            |
| `livenessProbe`                              | Configuration of the liveness probe.                                                                                         | `{}`            |
| `readinessProbe`                             | Configuration of the readiness probe.                                                                                        | `{}`            |
| `autoscaling.enabled`                        | Enable or disable Horizontal Pod Autoscaler.                                                                                 | `false`         |
| `autoscaling.minReplicas`                    | Minimum number of replicas.                                                                                                  | `1`             |
| `autoscaling.maxReplicas`                    | Maximum number of replicas.                                                                                                  | `100`           |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization percentage.                                                                                           | `80`            |
| `extraEnvVars`                               | Extra environment variables to be set on container                                                                           | `{}`            |
| `additionalVolumes`                          | Array of additional volumes to create                                                                                        | `[]`            |
| `additionalVolumeMounts`                     | Array of additional volumes to mount to container                                                                            | `[]`            |
| `nodeSelector`                               | Node selector configuration                                                                                                  | `{}`            |
| `tolerations`                                | Tolerations configuration                                                                                                    | `[]`            |
| `affinity`                                   | Affinity configuration                                                                                                       | `{}`            |

### MySQL parameters

| Name                                               | Description                                                         | Value                           |
| -------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------- |
| `mysql.enabled`                                    | Enable MySQL                                                        | `true`                          |
| `mysql.auth.rootPassword`                          | Root password for MySQL                                             | `lsdserver`                     |
| `mysql.auth.database`                              | Database name for MySQL                                             | `lsdserver`                     |
| `mysql.auth.username`                              | Username for MySQL                                                  | `lsdserver`                     |
| `mysql.auth.password`                              | Password for MySQL                                                  | `lsdserver`                     |
| `mysql.primary.persistence.size`                   | Size of persistent volume                                           | `1Gi`                           |
| `mysql.initdbScripts.mysql_db_setup_lsdserver.sql` | Initialization scripts containing required tables for Status server | `"Check script in values.yaml"` |
