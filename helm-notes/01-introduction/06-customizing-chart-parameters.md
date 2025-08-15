# Customizing Helm Chart Parameters

This guide explains how to customize Helm chart parameters when deploying applications, using a WordPress chart as an example. Helm uses settings from the chart’s `values.yaml` by default, but you can override these in various ways to suit your deployment needs.

## 1. Understanding the Default Configuration

- Helm charts rely on default values defined in the `values.yaml` file.
- For the Bitnami WordPress chart, example parameters include:

```yaml
wordpressUsername: user
wordpressPassword: ""
wordpressEmail: user@example.com
wordpressFirstName: WordPress user first name
wordpressBlogName: User's Blog!
```

These defaults are referenced in deployment manifests via templating, for example:

```yaml
env:
  - name: WORDPRESS_USERNAME
    value: {{ .Values.wordpressUsername | quote }}
  - name: WORDPRESS_BLOG_NAME
    value: {{ .Values.wordpressBlogName | quote }}
```

## 2. Customizing with `--set`

You can override one or more values directly during the install command:

```bash
helm install --set wordpressBlogName="Helm Tut" my-release bitnami/wordpress
```

To override multiple parameters, use multiple `--set` flags in a single command.

## 3. Using a Custom Values File

For multiple overrides, create a custom file (e.g., `custom-values.yaml`):

```yaml
wordpressBlogName: Helm Tutorials
wordpressEmail: user@example.com
```

Apply it with:

```bash
helm install --values custom-values.yaml my-release bitnami/wordpress
```

This method is clean, concise, and ideal for maintainable, repeatable deployments.

## 4. Modifying the Built-in `values.yaml`

For permanent changes, pull the chart locally:

```bash
helm pull --untar bitnami/wordpress
```

Edit the `values.yaml` file inside the extracted `wordpress/` directory, then install the modified chart:

```bash
helm install my-release ./wordpress
```

**Important**: Always test changes in a development environment before deploying to production.

## 5. Summary Table

| Method                     | Use Case                                | Example Command                                                                 |
|----------------------------|-----------------------------------------|---------------------------------------------------------------------------------|
| `--set`                    | Override a few parameters inline        | `helm install --set wordpressBlogName="Helm Tut" my-release bitnami/wordpress`   |
| Custom values file         | Override multiple values cleanly        | `helm install --values custom-values.yaml my-release bitnami/wordpress`         |
| Edit built-in `values.yaml`| Permanent, local customizations         | `helm install my-release ./wordpress`                                           |

## 6. Conclusion

You can customize Helm charts using the following approaches:

- **Inline overrides** with `--set` for quick, one-off tweaks.
- **Custom YAML files** for organized, repeatable overrides.
- **Editing the chart’s `values.yaml`** for deeper, persistent customizations.

These methods provide flexibility to tailor deployments to your workflow and environment.
```
