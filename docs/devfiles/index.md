# DevFile Variants

This project supports multiple DevFile configurations, each tailored for a
specific development workflow. Every variant lives under `devfiles/<name>/` and
ships with:

| File | Purpose |
|---|---|
| `devfile.yaml` | DevSpaces workspace definition |
| `Containerfile` | Container image build instructions (x86_64) |

Each variant has a corresponding PipelineRun under `pipelines/runs/` to build
and push its container image using OpenShift Pipelines.

## Available Variants

| Variant | Description | Status |
|---|---|---|
| [base](base.md) | Fedora 42 + cloud-native CLI tools | Stable |

## Creating a New Variant

1. Create a new directory under `devfiles/`:

    ```text
    devfiles/
    └── my-variant/
        ├── Containerfile
        └── devfile.yaml
    ```

2. Base your `Containerfile` on the base image or start from scratch with
   `fedora:42`.

3. Write a minimal `devfile.yaml`:

    ```yaml
    schemaVersion: 2.3.0
    metadata:
      name: devspaces-vibe-coding-my-variant
    components:
      - name: dev-tools
        container:
          image: image-registry.openshift-image-registry.svc:5000/<namespace>/devspaces-vibe-coding-my-variant:latest
          memoryLimit: 4Gi
          mountSources: true
    ```

4. Add a PipelineRun at `pipelines/runs/build-my-variant.yaml`.

5. Add documentation in `docs/devfiles/my-variant.md`.

6. Register the variant in `mkdocs.yml` under the `nav` section.

7. Open a pull request.

See the [Contributing](../contributing.md) guide for the full workflow.
