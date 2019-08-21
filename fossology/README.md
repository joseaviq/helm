# Fossology

[Fossology](fossology.org/) is source licensing scanning tool for opensource projects provided by the Linux Foundation.

## TL;DR;

```console
$ helm install . 
```


## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release .
```

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the Fossology chart and their default values.

|            Parameter             |                                  Description                                  |                           Default                            |
| -------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `image.registry`                 | Fossology image registry                                                      | `docker.io`                                                  |
| `image.repository`               | Fossology image name                                                          | `fossology/fossology`                                          |
| `image.tag`                      | image tag                                                           | `{TAG_NAME}`                                                 |
| `image.pullPolicy`               | Image pull policy                                                             | `IfNotPresent`                                               |
| `image.pullSecrets`              | Specify docker-registry secret names as an array                              | `[]` (does not add image pull secrets to deployed pods)      |
| `replicaCount`                   | Number of Pods to run                                               | `1`                                                          |
| `dbimage.enableTestDb`                | Deploy Postgress container(s)                                                   | `true`                                                       |
| `fossology.externalSecret`      | External secret t o be used for db configuration including host name                                                        | `nil`                                                        |
| `fossology.db_name`                | Database name to create                                                       | `fossology`                                          |
| `fossology.db_user`                | Database user to create                                                       | `fossy`                                               |
| `fossology.db_password`            | Password for the database                                                     | `fossy`       |
| `fossology.db_host`          | Host of the external database                                                 | `nil`                                                  |
| `service.type`                   | Kubernetes Service type                                                       | `ClusterIP`                                               |
| `service.port`                   | Service HTTP port                                                             | `80`                                                         |
| `ingress.enabled`                | Enable ingress controller resource                                            | `yes`                                                      |
| `ingress.annotations`            | Ingress annotations                                                           | see values file                                                         |
| `ingress.hosts[0].name`          | Hostname to your fossology installation                                       | `localhost`                                            |
| `ingress.hosts[0].path`          | Path within the url structure                                                 | `/repo`                                                          |
| `ingress.tls[]`        | TLS hosts                                                                     | []                    |
| `ingress.tls[0].secretName`      | TLS Secret (certificates)                                                     | `nil`                                        |
| `ingress.secrets[0].name`        | TLS Secret Name                                                               | `nil`                                                        |
| `ingress.secrets[0].certificate` | TLS Secret Certificate                                                        | `nil`                                                        |
| `ingress.secrets[0].key`         | TLS Secret Key                                                                | `nil`                                                        |
| `persistence.enabled`            | Enable persistence using PVC                                                  | `false`                                                       |
| `persistence.existingClaim`      | Enable persistence using an existing PVC                                      | `nil`                                                        |
| `persistence.accessMode`         | PVC Access Mode                                                               | `ReadWriteOnce`                                              |
| `persistence.size`               | PVC Storage Request                                                           | `10Gi`                                                       |
| `nodeSelector`                   | Node labels for pod assignment                                                | `{}`                                                         |
| `tolerations`                    | List of node taints to tolerate                                               | `[]`                                                         |
| `affinity`                       | Map of node/pod affinities                                                    | `{}`                                                         |


## Persistence

Persistent Volume Claims are used to keep the data across deployments. This is known to work in GCE, AWS, and minikube.
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.

You can use a ReadWriteOnce PVC to get persistence with 1 replica

If you want to scale make replicas you will need a ReadWriteMany PVC

## Using an external database

Sometimes you may want to have an external database rather than installing one inside your cluster, e.g. to use a managed database service, or use run a single database server for all your applications. To do this, the chart allows you to specify credentials for an external database under the [`externalDatabase` parameter](#configuration). You should also disable the MariaDB installation with the `mariadb.enabled` option. 

## Ingress

This chart provides support for ingress resources. If you have an
ingress controller installed on your cluster, such as [nginx-ingress](https://kubeapps.com/charts/stable/nginx-ingress)
or [traefik](https://kubeapps.com/charts/stable/traefik) you can utilize
the ingress controller to serve your application.

To enable ingress integration, please set `ingress.enabled` to `true`

### Hosts

Most likely you will only want to have one hostname that maps to this
installation, however, it is possible to have more than one
host.  To facilitate this, the `ingress.hosts` object is an array.

For each item, please indicate a `name`, `tls`, `tlsSecret`, and any
`annotations` that you may want the ingress controller to know about.

Indicating TLS will cause to generate HTTPS URLs, and
it will be connected to at port 443.  The actual secret that
`tlsSecret` references do not have to be generated by this chart.
However, please note that if TLS is enabled, the ingress record will not
work until this secret exists.

For annotations, please see [this document](https://github.com/kubernetes/ingress-nginx/blob/master/docs/annotations.md).
Not all annotations are supported by all ingress controllers, but this
document does a good job of indicating which annotation is supported by
many popular ingress controllers.

