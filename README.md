# Pod CleanUp

## Introduction

This is a Helm chart for Kubernetes that automates the cleanup of failed pods across all namespaces. It uses a `CronJob` to periodically execute the `kubectl delete pods` command to remove pods that are in the `Failed` state.

## Prerequisites

- Kubernetes 1.25+
- Helm 3.0+

## Installing on your cluster

The default values for this chart are defined in the `values.yaml` file. You can customize the settings by passing your own values during installation:

```bash
helm install pod-cleanup oci://ghcr.io/cubos/charts/pod-cleanup \
  --set podCleanup.schedule="0 2 * * *" \
  --version <version>
```

### Configurable Values

- `podCleanup.cleanupContainer.image.repository`: Repository of the cleanup container image (default: `bitnami/kubectl`).
- `podCleanup.cleanupContainer.image.tag`: Tag of the cleanup container image (default: `latest`).
- `podCleanup.schedule`: Cron schedule for the `CronJob` (default: `0 3 * * *`).
- `podCleanup.serviceAccount.annotations`: Annotations for the service account.

## Usage

This chart creates a `CronJob` that executes the command `kubectl delete pods --field-selector status.phase==Failed --all-namespaces` according to the configured cron schedule. Ensure that the service account has the necessary permissions to list and delete pods.

## Contributing

PRs are welcome! Please make sure to check ESLint and Prettier before submitting a PR.

By submitting a PR, you agree to license your work under the GNU Lesser General Public License v3.0.
