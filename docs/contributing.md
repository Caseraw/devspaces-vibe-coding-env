# Contributing

Contributions are welcome. This guide explains how to propose changes, add new
DevFile variants, and keep the project consistent.

## Development Workflow

1. **Fork** the repository and create a feature branch.
2. Make your changes.
3. Test locally (see below).
4. Open a **pull request** against `main`.

## Adding a New DevFile Variant

1. Create a directory under `devfiles/`:

    ```bash
    mkdir -p devfiles/my-variant
    ```

2. Add a `Containerfile` based on Fedora 42 (or extending the base image):

    ```dockerfile
    FROM image-registry.openshift-image-registry.svc:5000/<namespace>/devspaces-vibe-coding-base:latest
    # Add your customizations here
    ```

3. Add a minimal `devfile.yaml` following the
   [Devfile v2.3.0 specification](https://devfile.io/docs/2.3.0/what-is-a-devfile).

4. Add a PipelineRun at `pipelines/runs/build-my-variant.yaml`.

5. Add documentation at `docs/devfiles/my-variant.md`.

6. Register the page in `mkdocs.yml` under `nav > DevFiles`.

7. Test the image build on OpenShift:

    ```bash
    oc apply -f pipelines/tasks/
    oc apply -f pipelines/pipeline.yaml
    oc create -f pipelines/runs/build-my-variant.yaml
    ```

## Local Documentation Preview

```bash
pip install mkdocs-material
mkdocs serve
```

Then open `http://localhost:8000` in your browser.

## Commit Messages

Use clear, descriptive commit messages. Prefix with the area of change:

```text
docs: add Python variant documentation
devfiles: add Node.js 22 variant
pipelines: add PipelineRun for Python variant
ci: update pages workflow
```

## Code Review Checklist

- [ ] Containerfile builds successfully on x86_64
- [ ] DevFile validates against the Devfile v2.3.0 schema
- [ ] PipelineRun is provided for the new variant
- [ ] Documentation is added or updated
- [ ] GitHub Pages workflow passes
