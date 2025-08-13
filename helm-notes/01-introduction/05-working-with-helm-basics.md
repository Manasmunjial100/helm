Helm Basics Notes

Quick reference for using Helm, the Kubernetes package manager, to deploy and manage applications.

Key Helm Commands





View General Help: Check available commands and usage.

helm --help

Common actions: helm search, helm pull, helm install, helm list.



Manage Repositories:





Add a repository:

helm repo add bitnami https://charts.bitnami.com/bitnami



List repositories:

helm repo list



Update local chart cache:

helm repo update



Search for Charts:





Search Artifact Hub:

helm search hub wordpress



Search local repositories:

helm search repo wordpress

Deploying a WordPress Website





Add Bitnami Repository:

helm repo add bitnami https://charts.bitnami.com/bitnami

Output: "bitnami" has been added to your repositories.



Install WordPress Chart:

helm install my-release bitnami/wordpress





Creates a release named my-release.



Deploys WordPress in the default namespace.



Output includes DNS name (e.g., my-release-wordpress.default.svc.cluster.local:80).



List Releases:

helm list

Shows deployed releases, including name, namespace, status, and chart version.



Uninstall Release:

helm uninstall my-release

Removes all associated Kubernetes objects.

Tips





Chart Quality: Look for official or verified publisher badges on Artifact Hub for trusted charts.



Cost Efficiency: Use helm repo update to refresh chart info without re-adding repositories.