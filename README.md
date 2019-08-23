# Helm Chart for PostgreSQL
[![CircleCI](https://circleci.com/gh/cetic/helm-postgresql.svg?style=svg)](https://circleci.com/gh/cetic/helm-postgresql/tree/master) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) ![version](https://img.shields.io/github/tag/cetic/helm-postgresql.svg?label=release)

## Introduction

This [Helm](https://github.com/kubernetes/helm) chart installs [postgreSQL](https://www.postgresql.org/) in a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster 1.10+
- Helm 2.8.0+
- PV provisioner support in the underlying infrastructure.

## Installation

### Add Helm repository

```bash
helm repo add cetic https://cetic.github.io/helm-charts
helm repo update
```

### Configure the chart

The following items can be set via `--set` flag during installation or configured by editing the `values.yaml` directly (need to download the chart first).

#### Configure the way how to expose postgreSQL service:

- **ClusterIP**: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster.
- **NodePort**: Exposes the service on each Node’s IP at a static port (the NodePort). You’ll be able to contact the NodePort service, from outside the cluster, by requesting `NodeIP:NodePort`.
- **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer.

#### Configure the way how to persistent data: (TODO)

- **Disable**: The data does not survive the termination of a pod.
- **Persistent Volume Claim(default)**: A default `StorageClass` is needed in the Kubernetes cluster to dynamic provision the volumes. Specify another StorageClass in the `storageClass` or set `existingClaim` if you have already existing persistent volumes to use.

### Install the chart

Install the postgresql helm chart with a release name `my-release`:

```bash
helm install --name my-release cetic/postgresql
```

## Uninstallation

To uninstall/delete the `my-release` deployment:

```bash
helm delete --purge my-release
```

## Configuration

The following table lists the configurable parameters of the postgresql chart and the default values.

| Parameter                                                                   | Description                                                                                                        | Default                         |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------| ------------------------------- |
| **Image**                                                                   |
| `image.repository`                                                          | postgresql Image name                                                                                              | `postgres`                      |
| `image.tag`                                                                 | postgresql Image tag                                                                                               | `11.5`                          |
| `image.pullPolicy`                                                          | postgresql Image pull policy                                                                                       | `IfNotPresent`                  |
| `image.pullSecret`                                                          | postgresql Image pull secret                                                                                       | `nil`                           |
| **postgresql properties**                                                   |
| `postgresql.username`                                                       | postgresql username                                                                                                | `postgres`                      |
| `postgresql.password`                                                       | postgresql password                                                                                                | `true`                          |
| `postgresql.database`                                                       | postgresql database                                                                                                | `8080`                          |
| `postgresql.port`                                                           | postgresql port                                                                                                    | `null`                          |
| **Service**                                                                 |
| `service.type`                                                              | Type of service for postgresql frontend                                                                            | `CusterIP`                      |
| `service.loadBalancerIP`                                                    | LoadBalancerIP if service type is `LoadBalancer`                                                                   | `nil`                           |
| `service.clusterIP`                                                         | ClusterIP if service type is `ClusterIP`                                                                           | `nil`                           |
| `service.loadBalancerSourceRanges`                                          | Address that are allowed when svc is `LoadBalancer`                                                                | `[]`                            |
| `service.annotations`                                                       | Service annotations                                                                                                | `{}`                            |
| **Persistence (TODO)**                                                      |
| `persistence.enabled`                                                       | Use persistent volume to store data                                                                                | `false`                         |
| `persistence.storageClass`                                                  | Storage class name of PVCs                                                                                         | `standard`                      |
| `persistence.accessMode`                                                    | ReadWriteOnce or ReadOnly                                                                                          | `[ReadWriteOnce]`               |
| `persistence.size`                                                          | Size of persistent volume claim                                                                                    | `5Gi`                           |
| **ReadinessProbe**                                                          |
| `readinessProbe`                                                            | Rediness Probe settings                                                                                            | `nil`                           |
| **LivenessProbe**                                                           |
| `livenessProbe`                                                             | Liveness Probe settings                                                                                            | `nil`                           |
| **Resources**                                                               |
| `resources`                                                                 | Pod resource requests and limits for logs                                                                          | `{}`                            |
| **nodeSelector**                                                            |
| `nodeSelector`                                                              | Node labels for pod assignment                                                                                     | `{}`                            |
| **tolerations**                                                             |
| `tolerations`                                                               | Tolerations for pod assignment                                                                                     | `[]`                            |

## Why this PostgreSQL Helm Chart?

* postgres official Docker Image.
* TODO

## Contributing

Feel free to contribute by making a [pull request](https://github.com/cetic/helm-postgresql/pull/new/master).

Please read the official [Contribution Guide](https://github.com/helm/charts/blob/master/CONTRIBUTING.md) from Helm for more information on how you can contribute to this Chart.

## License

[Apache License 2.0](/LICENSE)