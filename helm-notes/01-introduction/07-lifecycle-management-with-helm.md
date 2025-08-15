
```markdown
# Helm Lifecycle Management

This guide covers how Helm simplifies Kubernetes application lifecycle management, including installing, upgrading, rolling back, and tracking releases, using NGINX and WordPress as examples.

## What Is a Release?

A Helm *release* is a deployed instance of a chart, representing a collection of Kubernetes objects. Helm tracks these objects, enabling independent management of multiple releases from the same chart:

```bash
helm install my-site bitnami/wordpress
helm install my-second-site bitnami/wordpress
```

## Installing a Specific Chart Version

To install a specific version of a chart, use the `--version` flag. For example, install NGINX version 7.1.0:

```bash
helm install nginx-release bitnami/nginx --version 7.1.0
```

This creates a release named `nginx-release` with associated Kubernetes objects (e.g., Pods, Services).

## Verifying the Deployed Version

Check the running version by inspecting the Pod:

```bash
kubectl get pods
# Example output: nginx-release-687cdd5c75-ztn2n  0/1  ContainerCreating  0  13s

kubectl describe pod nginx-release-687cdd5c75-ztn2n
# Example output: Image: docker.io/bitnami/nginx:1.19.2-debian-10-r28
```

This confirms the Pod runs NGINX 1.19.2.

## Upgrading a Release

To upgrade to a newer chart version, use `helm upgrade`:

```bash
helm upgrade nginx-release bitnami/nginx
```

This upgrades the release to a newer version (e.g., NGINX 1.21.4, chart 9.5.13). Verify the new Pod:

```bash
kubectl get pods
kubectl describe pod <new-pod-name>
```

## Tracking Release History

Helm tracks release history. List releases to see the current revision:

```bash
helm list
# Example output:
# NAME          NAMESPACE REVISION STATUS   CHART        APP VERSION
# nginx-release default   2        deployed nginx-9.5.13 1.21.4
```

View detailed history:

```bash
helm history nginx-release
# Example output:
# REVISION UPDATED                  STATUS     CHART        APP VERSION DESCRIPTION
# 1        Mon Nov 15 19:20:51 2021 superseded nginx-7.1.0  1.19.2      Install complete
# 2        Mon Nov 15 19:25:55 2021 deployed   nginx-9.5.13 1.21.4      Upgrade complete
```

## Rolling Back Upgrades

To revert to a previous revision, use `helm rollback`:

```bash
helm rollback nginx-release 1
```

This restores the configuration of revision 1, creating a new revision (e.g., 3) to maintain history.

## External Data Caution

Helm manages configurations but **not** external data (e.g., persistent volumes, databases). Use Chart Hooks or backup solutions to handle data during rollbacks.

## Handling Upgrade Parameters

Some upgrades require additional parameters (e.g., credentials for WordPress). If missing, you may see errors:

```bash
helm upgrade wordpress-release bitnami/wordpress
# Error: PASSWORDS ERROR: You must provide current passwords
```

Retrieve existing credentials and include them:

```bash
export WORDPRESS_PASSWORD=$(kubectl get secret --namespace default wordpress-release -o jsonpath="{.data.wordpress-password}" | base64 --decode)
helm upgrade wordpress-release bitnami/wordpress --set wordpressPassword=$WORDPRESS_PASSWORD
```

See [Bitnami documentation](https://docs.bitnami.com/general/how-to/troubleshoot-helm-chart-issues/#credential-errors-while-upgrading-chart-releases) for details.

## Summary

| Action          | Command Example                                   | Purpose                              |
|-----------------|--------------------------------------------------|--------------------------------------|
| Install         | `helm install nginx-release bitnami/nginx --version 7.1.0` | Deploy a specific chart version       |
| Upgrade         | `helm upgrade nginx-release bitnami/nginx`        | Update to a newer chart version       |
| History         | `helm history nginx-release`                     | View release revisions               |
| Rollback        | `helm rollback nginx-release 1`                  | Revert to a previous revision        |

## Conclusion

Helm simplifies Kubernetes application management by:
- Tracking releases and their Kubernetes objects.
- Enabling seamless upgrades and rollbacks.
- Maintaining detailed release history.