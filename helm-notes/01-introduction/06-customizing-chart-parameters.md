Customizing Helm Chart Parameters
This guide explains how to customize Helm chart parameters when deploying applications—using a WordPress example. By default, Helm uses settings from the chart’s values.yaml, but you can override these in various ways to suit your deployment needs.

1. Understanding the Default Configuration
Helm charts use default values defined in values.yaml.
For example, the WordPress chart includes parameters like:
```yaml
wordpressUsername: user
wordpressPassword: “”
wordpressEmail: user@example.com
wordpressFirstName: WordPress user first name
wordpressBlogName: User’s Blog!
These defaults are referenced in deployment manifests via templating:

yaml
Copy
Edit
env:

name: WORDPRESS_USERNAME
value: {{ .Values.wordpressUsername | quote }}
name: WORDPRESS_BLOG_NAME
value: {{ .Values.wordpressBlogName | quote }}
Customizing with —set
Override one or more values directly in the install command:
bash
Copy
Edit
helm install —set wordpressBlogName=”Helm Tut” my-release bitnami/wordpress
Use multiple —set flags to override several parameters at once.

Using a Custom Values File
For multiple overrides, create a file (e.g., custom-values.yaml):
yaml
Copy
Edit
wordpressBlogName: Helm Tutorials
wordpressEmail: user@example.com
Use it with:

bash
Copy
Edit
helm install —values custom-values.yaml my-release bitnami/wordpress
This method is clean, concise, and maintainable.

Modifying the Built-in values.yaml
For permanent changes, pull the chart locally:
bash
Copy
Edit
helm pull —untar bitnami/wordpress
Edit the values.yaml inside the extracted wordpress/ directory.

Install from your modified chart:

bash
Copy
Edit
helm install my-release ./wordpress
Important: Always test changes in a dev environment before pushing to production.

Summary Table
Method Use Case Example Command
—set Override a few parameters inline helm install —set wordpressBlogName=”Helm Tut” my-release bitnami/wordpress
Custom values file Override multiple values cleanly helm install —values custom-values.yaml my-release bitnami/wordpress
Edit built-in values.yaml Permanent, local customizations helm install my-release ./wordpress

Conclusion
You can customize Helm charts through:

Inline overrides using —set for quick tweaks.

Custom YAML files for organized and repeatable overrides.

Editing the chart itself for deeper, persistent customization.

These approaches give you flexibility tailored to your workflow and environment.