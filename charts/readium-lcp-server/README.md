# Readium License Server

## Parameters

### Common parameters

| Name               | Description                                                                           | Value |
| ------------------ | ------------------------------------------------------------------------------------- | ----- |
| `nameOverride`     | String to partially override lcpserver.name template (will maintain the release name) | `""`  |
| `fullnameOverride` | String to fully override lcpserver.name template                                      | `""`  |
| `replicaCount`     | Desired number of replicas                                                            | `1`   |

### Image configuration

| Name               | Description                                      | Value                                                                    |
| ------------------ | ------------------------------------------------ | ------------------------------------------------------------------------ |
| `image.repository` | Image repository                                 | `registry.gitlab.com/skczu/elektronicke-skripta/readium-stack/lcpserver` |
| `image.pullPolicy` | Image pull policy                                | `IfNotPresent`                                                           |
| `image.tag`        | Image tag                                        | `latest`                                                                 |
| `imagePullSecrets` | Specify docker-registry secret names as an array | `[]`                                                                     |

### Licence server parameters

| Name                    | Description                                                                                                                                                             | Value           |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| `profile`               | Value of LCP profile. Allowed values are "basic" or "1.0"                                                                                                               | `basic`         |
| `configDirectory`       | Application directory for mounting configuration files and certificate                                                                                                  | `/app`          |
| `auth.username`         | Username for API requests authentication                                                                                                                                | `readium`       |
| `auth.password`         | Password for API requests authentication                                                                                                                                | `readium`       |
| `auth.existingSecret`   | Name of existing secret containing key "htpasswd" with username and password encrypted using htpasswd utility                                                           | `""`            |
| `auth.authfileMountDir` | Name of directory where to mount htpasswd file                                                                                                                          | `/app/htpasswd` |
| `lcp.host`              | Public server hostname                                                                                                                                                  | `"0.0.0.0"`     |
| `lcp.port`              | Listening port. Default to service.port parameter                                                                                                                       | `""`            |
| `lcp.public_base_url`   | URL used by the Status Server and the Frontend Server to communicate with this License server. Automatically constructed using name of created service and service port | `""`            |
| `lcp.readonly`          | Default to false                                                                                                                                                        | `""`            |
