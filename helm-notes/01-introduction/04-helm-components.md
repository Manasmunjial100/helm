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


# ⚙️ Helm Charts and tempting
Helm charts bundle not only Kubernetes manifest files but also powerful templating capabilities that support flexibility and customization. Consider a simple HelloWorld application running an Nginx web server. This application uses two primary Kubernetes objects—a Deployment and a Service. The deployment template uses templating to substitute values defined in a separate configuration file.

Below is an example of a basic Helm chart, organized into distinct files:

# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app: hello-world
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: nginx
          image: {{ .Values.image.repository }}
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
# values.yaml
replicaCount: 1
image:
  repository: nginx
In this example, the values.yaml file provides dynamic configuration values (such as replica count and image repository), allowing you to customize deployments effortlessly without altering the core chart templates.