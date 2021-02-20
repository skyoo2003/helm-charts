# Digdag

[Digdag](https://github.com/treasure-data/digdag) is a simple tool that helps you to build, run, schedule, and monitor complex pipelines of tasks. It handles dependency resolution so that tasks run in order or in parallel.

## Prerequisites

* Kubernetes >= 1.15
* Helm >= 3.1

## Install the chart

1. Add the Helm repository:
  ```console
  helm repo add skyoo2003 https://skyoo2003.github.io/helm-charts
  ```
2. Run the following command, providing a name for your release:

  ```console
  helm upgrade --install my-release skyoo2003/digdag
  ```

These commands deploy Airflow on the Kubernetes cluster in the default configuration. The [Configure the chart](#configure-the-chart) section lists the parameters that can be configured during installation.

Tip: List all releases using `helm list`

## Uninstall the chart

To uninstall the my-release deployment, use the following command:

  ```console
  helm uninstall my-release
  ```

This command removes all the Kubernetes components associated with the chart and deletes the release.

## Configure the chart

The following table lists configurable parameters, their descriptions, and their default values stored in `values.yaml`.

| Parameter | Description | Default |
|---|---|---|
| image.repository | Image repository url. | docker.io/szyn/docker-digdag |
| image.tag | Image tag. | 0.9.42 |
| image.pullPolicy | Image pull policy. | IfNotPresent |
| image.command | Image entrypoint. | nil |
| image.args | Image entrypoint's arguments. | ["server", "--config", "/opt/etc/digdag/server.properties"] |
| replicaCount | Number of Digdag replicas to deploy. | 3 |
| updateStrategy | Update strategy to deploy. See [link](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy) for details. | {type: RollingUpdate} |
| terminationGracePeriodSeconds | Duration in seconds a Digdag pod needs to terminate gracefully. | 30 |
| shareProcessNamespace | Whether to share process namespace for pods | false |
| hostAliases | Pod's host aliases (/etc/hosts) | [] |
| volumes | Extra volumes | [] |
| volumeMounts | Extra volume mounts | [] |
| envs | Extra environment variables | [] |
| sidecars | Additional sidecar containers | [] |
| podAnnotations | Pod annotations. See [link](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for details. | {} |
| podSecurityContext | Pod Security Context. See [link](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod) for details. | {enabled: false, fsGroup: 1000} |
| securityContext | Pod containers' Security Context. See [link](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container) for details. | {enabled: false, runAsNonRoot: true, runAsUser: 1000} |
| serviceAccount | Service account for Digdag to use. See [link](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/) for details. | {create: false, annotations: {}, name: ""} |
| rbac | Role-based access control (RBAC) configuration | {create: false} |
| nodeSelector | Node labels for pod assignment. | |
| tolerations | Tolerations for pod assignment | |
| affinity | Affinity for pod assignment | |
| service | Digdag Service | |
| jmx | Digdag JMX configuration | |
| ingress | Digdag Ingress | |
| resources | Digdag resources | |
| livenessProbe | The container's liveness probe configuration | {enabled: true, initialDelaySeconds: 60, periodSeconds: 10, timeoutSeconds: 5, successThreshold: 1, failureThreshold: 6} |
| readinessProbe | The container's readiness probe configuration | {enabled: true, initialDelaySeconds: 60, periodSeconds: 10, timeoutSeconds: 5, successThreshold: 1, failureThreshold: 6} |
| configuration | Digdag configuration | See the `values.yaml` file for details. |
| waitForIt | InitContainer for waiting PostgreSQL is ready. | See the `values.yaml` file for details. |
| metrics | Digdag jmx exporter configuration | See the `values.yaml` file for details. |
| autoscaling | Horizontal Pod Autoscaler configuration. | {enabled: false, maxReplicas: 10, minReplicas: 2, cpuUtilization: 50, memoryUtilization: 50} |
| pdb | Pod Disruption Budget configuration | {enabled: false, minAvailable: 1, annotations: {}} |
| postgresql-ha | Postgresql HA Subchart's configuration. See [link](https://github.com/bitnami/charts/tree/master/bitnami/postgresql-ha) for details. | |
| postgresql | Postgresql Subchart's configuration. See [link](https://github.com/bitnami/charts/tree/master/bitnami/postgresql) for details. | |
| testFramework.enabled | For testing Helm charts | false |

To configure the chart, do either of the following:

* Specify each parameter using the `--set key=value[,key=value]` argument to `helm upgrade --install`. For example:

  ```console
  helm upgrade --install my-release \
    --set metrics.enabled=true,metrics.serviceMonitor.enabled=true \
      skyoo2003/digdag
  ```

  This command enables Metricss service and ServiceMonitor.

* Provide a YAML file that specifies the parameter values while installing the chart. For example, use the following command:

  ```console
  helm upgrade --install my-release -f values.yaml skyoo2003/digdag
  ```

Tip: Use the default [values.yaml](values.yaml).

For information about running Digdag in Docker, see the [full image documentation](https://hub.docker.com/r/szyn/docker-digdag).
