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

If the password is generated you will need to save this secret before you upgrade. You can do this using the following commands. **If you do not do this, you will not be able to retrieve the previous secrets**.

```
echo BETY_PASSWORD=$(kubectl get secrets betydb -o json | jq -r '.data.betyPassword' | base64 -d)
echo BETY_SECRETKEY=$(kubectl get secrets betydb -o json | jq -r '.data.secretKey' | base64 -d)
echo POSTGRESQL_PASSWORD=$(kubectl get secrets betydb-postgresql -o json | jq -r '.data."postgresql-password"' | base64 -d)
```
> **Tip**: List all secrets in the default namespace

**If you wanted to have it into particular namespace, you can try below sample one**

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

## Configuration

A YAML file that specifies the values for the necessary parameters can be provided while installing the chart. For example,

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

### 0.5.0
- initial release of the BETY helm chart.
- build on bety 5.4.1
