# Bety
Bety is a Web-interface to the Biofuel Ecophysiological Traits and Yields Database (used by [PEcAn](https://pecanproject.github.io/) and [TERRA REF](https://terraref.org/))

## Prerequisites
- [Kubernetes](https://kubernetes.io/)
- [Helm3](https://helm.sh/)

## Installation

> Before installing chart, make sure you enable NCSA repository.

```
helm repo add ncsa https://opensource.ncsa.illinois.edu/charts/
```

To install the chart with the release name my-release:

```
helm install my-release ncsa/betydb
```
> **Tip**: List all releases using `helm list`

The command deploys Betydb on the Kubernetes cluster in the default namespace and configuration.

**To install the chart in a specific namespace:**

```
kubectl create namespace my-namepsace
helm install my-release ncsa/betydb -n my-namepsace
```

If the password is generated you will need to save this secret before you upgrade. You can do this using the following commands. **If you do not do this, you will not be able to retrieve the previous secrets**.

```
echo BETY_PASSWORD=$(kubectl get secrets betydb -o json | jq -r '.data.betyPassword' | base64 -d)
echo BETY_SECRETKEY=$(kubectl get secrets betydb -o json | jq -r '.data.secretKey' | base64 -d)
echo POSTGRESQL_PASSWORD=$(kubectl get secrets betydb-postgresql -o json | jq -r '.data."postgresql-password"' | base64 -d)
```
> **Tip**: List all secrets in the default namespace

**If you wanted to have it into the particular namespace, you can try below sample one**

```
echo BETY_PASSWORD=$(kubectl get secrets betydb -n your_namespace -o json | jq -r '.data.betyPassword' | base64 -d)
```

> **Tip**: Make sure you have created your own namespace before listing out the secrets ```kubectl create namespace your_namespace```.

## Upgrading BETY

You can also upgrade and use the secrets retrieved.

```
helm upgrade betydb ncsa/betydb \
    --set betyPassword="${BETY_PASSWORD}" \
    --set secretKey="${BETY_SECRETKEY}" \
    --set postgresql.postgresqlPassword="${POSTGRESQL_PASSWORD}"
```

## Parameters
The following table lists the configurable parameters of the Bety chart and their default values per section/component:

### Common parameters

| Parameter                 | Description                                                              | Default                                                 |
|---------------------------|--------------------------------------------------------------------------|---------------------------------------------------------|
| `nameOverride`            | String to partially override bety.fullname                               | `nil`                                                   |
| `fullnameOverride`        | String to fully override bety.fullname                                   | `nil`                                                   |

### Bety parameters

| Parameter                 | Description                                                                              | Default                                                 |
|---------------------------|------------------------------------------------------------------------------------------|---------------------------------------------------------|
| `image.registry`                     | Bety image registry                                                           | `docker.io`                                             |
| `image.repository`                   | Bety image name                                                               | `pecan/bety`                                            |
| `image.tag`                          | Bety image tag                                                                | `null`                                                  |
| `image.pullPolicy`                   | Bety image pull policy                                                        | `IfNotPresent`                                          |
| `image.pullSecrets`                  | Specify docker-registry secret names as an array                              | `[]` (does not add image pull secrets to deployed pods) |
| `replicaCount`                       | Number of Bety Pods to run                                                    | `1`                                                     |
| `nodeSelector`                       | Node labels for pod assignment                                                | `{}` (evaluated as a template)                          |
| `tolerations`                        | Tolerations for pod assignment                                                | `[]` (evaluated as a template)                          |
| `affinity`                           | Affinity for pod assignment                                                   | `{}` (evaluated as a template)                          |
| `service.type`                       | Kubernetes Service type                                                       | `ClusterIP`                                             |
| `service.port`                       | Service HTTP port                                                             | `8000`                                                    |
| `betyUser`                           | User value for bety.user                                                      | `bety`                                                  |
| `betyPassword`                       | Password value for bety.password                                              | `bety`                                                  |
| `betyDatabase`                       | Name of the database for bety.database                                        | `bety`                                                  |

### OpenShift/Kubernetes parameters

| Parameter                         | Description                                              | Default                        |
|-----------------------------------|----------------------------------------------------------|--------------------------------|
| `serviceAccount.enabled`          | Enable creation and use of a deployment service account  | false                          |
| `serviceAccount.name`             | Add a serviceAccountName to the deployment               | ``                             |
| `serviceAccount.annotations`      | Add annotations to the serviceAccount                    | {}                             |

### Ingress parameters

| Parameter                         | Description                                              | Default                        |
|-----------------------------------|----------------------------------------------------------|--------------------------------|
| `ingress.enabled`                 | Enable ingress controller resource                       | `false`                        |
| `ingress.host`                    | Default host for the ingress resource                    | `[]` (evaluated as a template) |
| `ingress.tls`                     | TLS configuration for the hostnames to be covered        | `false`                        |
| `ingress.annotations`             | Ingress annotations                                      | `[]` (evaluated as a template) |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,
```
helm install mt-release ncsa/betydb \
    --set betyPassword="xxxx" \
    --set secretKey="xxxx" \
    --set postgresql.postgresqlPassword="xxxx"
```

The above command sets the bety password, secret, postgresql password to `xxxx`,`xxxx`, and `xxxx` respectively.

Alternatively, a YAML file that specifies the values for the necessary parameters can be provided while installing the chart. For example,

```
helm install --name my-release -f values.yaml .
```

> **Tip**: You can use the default values.yaml

## Uninstalling the Chart
To uninstall/delete the my-release deployment:

```
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## ChangeLog

### 0.5.4
- back to hooks since job completion requires RBAC role

### 0.5.3
- need to check for table before start bety application

### 0.5.2
- use new check image to use PG environment variables
- add-user and load-db are now jobs, not hooks (prevent timeout issues)

### 0.5.1
- update README to describe values
- fix left over when initializing from URL
- fix binami url change

### 0.5.0
- initial release of the BETY helm chart.
- build on bety 5.4.1
