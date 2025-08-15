
## Overview

Helm charts automate Kubernetes application deployment by packaging resources into templates with configurable values. This guide demonstrates creating a chart for an NGINX application with two replicas and a NodePort service.

## Creating a Simple "Hello World" Chart

### Kubernetes Manifests
Define the following for an NGINX-based application:
- **Service**: Exposes the application via NodePort.
- **Deployment**: Runs two NGINX replicas.

Example manifests:
```yaml
# Service (service.yaml)
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
```
```yaml
# Deployment (deployment.yaml)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-world
          image: nginx
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
```

## Scaffolding the Chart

Use Helm’s CLI to create a chart structure:
```bash
helm create nginx-chart
```
This generates:
```bash
nginx-chart/
  ├── charts/
  ├── templates/
  ├── Chart.yaml
  ├── values.yaml
  ├── LICENSE
  ├── README.md
```
- Replace default templates in `templates/` with your `service.yaml` and `deployment.yaml`.
- Remove unnecessary files (e.g., `hpa.yaml`, `ingress.yaml`) for simplicity.

## Updating Chart Metadata

Edit `Chart.yaml` to add descriptive metadata:
```yaml
apiVersion: v2
name: nginx-chart
description: Basic NGINX website
type: application
version: 0.1.0
appVersion: "1.16.0"
maintainers:
  - name: John Smith
    email: john@example.com
```

## Avoiding Name Conflicts with Templating

Hardcoded names (e.g., `hello-world`) cause conflicts if multiple releases are installed. Use Helm’s templating to create dynamic names based on the release name:
```yaml
# templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-svc
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app: hello-world
```
```yaml
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-nginx
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
        - name: hello-world
          image: "{{ .Values.image }}"
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
```

Install multiple releases without conflicts:
```bash
helm install hello-world-1 ./nginx-chart
helm install hello-world-2 ./nginx-chart
```
Verify with:
```bash
kubectl get deployment
# Example output:
# NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
# hello-world-1-nginx   1/2     2            1           8s
# hello-world-2-nginx   0/2     2            0           4s
```

## Configuring Values

Define default configurations in `values.yaml`:
```yaml
replicaCount: 2
image: nginx
```
For more complex setups, use a dictionary:
```yaml
replicaCount: 2
image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "1.16.0"
```

Update the deployment template to reference these:
```yaml
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-nginx
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
        - name: hello-world
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
```

Override values during installation:
```bash
helm install hello-world-1 ./nginx-chart --set replicaCount=3 --set image=nginx:1.18.0
```

## Summary Table

| Task                   | Command/Action                                   | Purpose                                      |
|------------------------|-------------------------------------------------|----------------------------------------------|
| Scaffold Chart         | `helm create nginx-chart`                       | Generate chart structure                     |
| Update Metadata        | Edit `Chart.yaml`                               | Add descriptive chart details                |
| Templating             | Use `{{ .Release.Name }}` in templates          | Avoid name conflicts across releases         |
| Configure Values       | Edit `values.yaml` or use `--set`               | Customize deployment settings                |

## Conclusion

Helm charts simplify Kubernetes deployments by:
- Automating resource creation with templates.
- Using dynamic naming to prevent conflicts.
- Allowing customizable configurations via `values.yaml` or `--set`.

