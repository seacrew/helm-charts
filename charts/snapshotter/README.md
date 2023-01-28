# Snapshotter 
A simple Helm Chart to manage your `VolumeSnapshots` with a Kubernetes `CronJob` executing a shell script.

## Introduction
This chart bootstraps a cronjob on a schedule to create `VolumeSnapshots` for your precious `PersistentVolumeClaims` and automatically deletes them after a specified retention time as well.

## Installing the chart
```bash
helm repo add seacrew https://seacrew.github.io/helm-charts
helm repo update
helm install snapshotter seacrew/snapshotter -f override.yaml
```

## How to use it

Just create your override.yaml file and specify your schedule, retention time and pvcs as a comma separated list.

```yaml
snapshotter:
  schedule: "0 1 * * *" # daily at 1 am
  volumeClaims: "your-amazing-pvc,your-other-pvc"
  retentionInDays: 7 # retention time of 7 days
```

## Values

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `snapshotter.schedule`        | `CronJob` schedule to create `VolumeSnapshots`        | `0 0 * * *`       |
| `snapshotter.volumeClaims`    | Comma separated list of `PersistentVolumeClaims`      | `None`            |
| `snapshotter.retentionInDays` | Retention of `VolumeSnapshots` in days                | `7`               |
| `snapshotter.restartPolicy`   | Restart Policy if `CronJob` fails                     | `never`           |
| `image.repository`            | Image used for `CronJob`                              | `bitnami/kubectl` |
| `image.pullPolicy`            | Image pull policy                                     | `IfNotPresent`    |
| `image.tag`                   | `CronJob` image tag                                   | `latest`          |
| `imagePullSecrets`            | imagePullSecrets to use for private repository        | `None`            |
| `serviceAccount.create`       | If set to true, create a serviceAccount               | `true`            |
| `serviceAccount.annotations`  |  Additional serviceAccount annotations                | `None`            |
| `serviceAccount.name`         | Custom name for the serviceAccount to create/use      | `Default`         |
| `rbac.create`                 | If set to true, create rbac resources                 | `true`            |
| `rbac.annotations`            | Additional rbac annotations                           | `None`            |
| `rbac.roleName`               | Custom name for the `Role` to create/use              | `Default`         |
| `rbac.rolebindingName`        | Custom name for the `RoleBinding` to create/use       | `Default`         |
| `rbac.rules`                  | `Role` rules necessary to manage `VolumeSnapshots`    | `Full CRUD`       |
| `podSecurityContext`          | Pod Security Context                                  | `None`            |
| `securityContext`             | Security Context                                      | `None`            |
| `nodeSelector`                | Node labels for pod assignment                        | `None`            |
| `tolerations`                 | List of node taints to tolerate                       | `None`            |
| `affinity`                    | Node / Pod affinities                                 | `None`            |
