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

### Additional Tips for Consistent Formatting
- **Use a Monospace Font in Your Editor**: This helps align table columns visually, making it easier to spot errors.
- **Check Line Endings**: Ensure your editor uses LF (Unix-style) line endings, as GitHub prefers this. In VS Code, you can set this in the bottom-right corner of the editor.
- **Validate with GitHub Desktop or Online Tools**: If you’re unsure about local rendering, paste the content into GitHub’s “Create new file” interface (in your repository) to preview before committing.
- **Avoid Rich-Text Editors**: Tools like Microsoft Word or Google Docs can add hidden formatting. Stick to plain-text editors like VS Code, Notepad++, or Atom.

### If the Table Still Doesn’t Render
If the table still appears empty or broken after copying the above content:
1. **Check for Extra Spaces or Tabs**:
   - Open the `.md` file in a text editor and ensure there are no extra spaces or tabs in the table rows.
   - Example of a broken table:
     ```markdown
     | Method | Use Case | Example Command |  <- Extra space here
     |--------|----------|-----------------|
     | `--set`| Override | `helm install`  |  <- Misaligned pipes
     ```
   - Fix by aligning pipes and removing extra spaces.

2. **Test a Minimal Table**:
   - Create a new `.md` file with just the table to isolate the issue:
     ```markdown
     | A | B | C |
     |---|---|---|
     | 1 | 2 | 3 |
     ```
   - If this renders correctly, the issue is specific to the original table’s content.

3. **Push to GitHub and Check**:
   - Commit and push the file to GitHub, then view it in your repository. GitHub’s renderer is reliable for GFM.
   - If it works on GitHub but not locally, your local viewer may not support GFM tables.

4. **Share Specific Issues**:
   - If the problem persists, let me know:
     - Whether the table is empty, misaligned, or not rendering at all.
     - The editor or viewer you’re using locally.
     - If it’s failing on GitHub, share a link to the file or a screenshot.

### Alternative Format (If Tables Continue to Cause Issues)
If you prefer to avoid tables due to persistent rendering issues, I can convert the table into a bulleted list. Here’s an example:

```markdown
## 5. Summary

- **Method**: `--set`
  - **Use Case**: Override a few parameters inline
  - **Example Command**: `helm install --set wordpressBlogName="Helm Tut" my-release bitnami/wordpress`
- **Method**: Custom values file
  - **Use Case**: Override multiple values cleanly
  - **Example Command**: `helm install --values custom-values.yaml my-release bitnami/wordpress`
- **Method**: Edit built-in `values.yaml`
  - **Use Case**: Permanent, local customizations
  - **Example Command**: `helm install my-release ./wordpress`
```

Let me know if you want to use this list format instead or if you need help with anything else (e.g., Git commands, specific editor settings, or further troubleshooting)!