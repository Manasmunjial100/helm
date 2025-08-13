# ⚙️ Helm Key Components

1. **Helm CLI**  
   The Helm command-line utility runs on your local machine, enabling you to install charts, upgrade releases, roll back changes, and perform other operations.

2. **Charts**  
   Charts are packages containing all the instructions Helm needs to create the Kubernetes objects required by an application.  
   They serve as reusable deployment packages and are available publicly from various repositories.

3. **Releases**  
   A release is created when a chart is deployed to your cluster.  
   It represents a single installation of an application based on a Helm chart.  
   Each time you perform an action — such as an upgrade or configuration change — a new revision (or snapshot) is generated, enabling independent management of multiple application versions.

4. **Metadata**  
   Helm stores release metadata, including chart details and revision history, as Secrets within your Kubernetes cluster.  
   This ensures that the deployment history remains accessible to everyone working on the cluster.


![Helm Components Diagram](../images/helm-components-chart-repository-diagram.jpg)