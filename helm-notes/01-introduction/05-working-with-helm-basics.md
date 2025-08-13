# 📘 Helm Basics Notes

Quick reference for using Helm to deploy and manage Kubernetes applications.

---

```bash
# 🔹 View Help
helm --help

# 📦 Manage Repositories
## Add a repository
helm repo add bitnami https://charts.bitnami.com/bitnami

## List repositories
helm repo list

## Update chart cache
helm repo update

# 🔍 Search Charts
## Search Artifact Hub
helm search hub wordpress

## Search local repositories
helm search repo wordpress

# 🚀 Deploying WordPress
## Add Bitnami Repository
helm repo add bitnami https://charts.bitnami.com/bitnami

## Install WordPress Chart
helm install my-release bitnami/wordpress
# Creates release 'my-release' in default namespace
# Output shows DNS name, e.g., my-release-wordpress.default.svc.cluster.local:80

# 📋 List Releases
helm list

# 🗑️ Uninstall Release
helm uninstall my-release
