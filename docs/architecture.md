# Project Architecture

## Repository Layout

```text
devspaces-vibe-coding-env/
├── .devcontainer/              # GitHub Codespaces / VS Code Dev Containers
│   ├── Containerfile
│   ├── devcontainer.json
│   └── files/
├── .github/
│   └── workflows/
│       └── pages.yml           # Deploy documentation to GitHub Pages
├── devfiles/                   # DevFile variants and their container images
│   └── base/
│       ├── Containerfile       # Fedora-based container image (x86_64)
│       └── devfile.yaml        # DevSpaces workspace definition
├── pipelines/                  # OpenShift Pipelines (Tekton) resources
│   ├── tasks/
│   │   ├── git-clone.yaml      # Task: clone a Git repository
│   │   └── buildah.yaml        # Task: build and push with buildah
│   ├── pipeline.yaml           # Pipeline: build-devfile-image
│   ├── pvc.yaml                # Shared workspace PVC
│   └── runs/
│       └── build-base.yaml     # PipelineRun for the base image
├── docs/                       # MkDocs documentation source
│   ├── index.md
│   ├── getting-started.md
│   ├── architecture.md
│   ├── pipelines.md
│   ├── contributing.md
│   └── devfiles/
│       ├── index.md
│       └── base.md
├── mkdocs.yml                  # Documentation site configuration
├── .gitignore
├── LICENSE
└── README.md
```

## Design Principles

### Separation of Concerns

| Directory | Responsibility |
|---|---|
| `devfiles/` | DevSpaces workspace definitions and their container images |
| `pipelines/` | Tekton resources for building and pushing images on OpenShift |
| `.devcontainer/` | VS Code / GitHub Codespaces compatibility |
| `docs/` | User-facing documentation |
| `.github/workflows/` | GitHub Pages deployment |

### DevFile-per-Variant

Each variant under `devfiles/<name>/` is self-contained with:

- `Containerfile` — defines the container image (x86_64)
- `devfile.yaml` — defines the DevSpaces workspace

When creating a DevSpaces workspace, specify the devfile path:

```text
https://<devspaces-host>/#<repo-url>?devfilePath=devfiles/<variant>/devfile.yaml
```

### OpenShift Pipelines (Tekton)

Container images are built directly on the OpenShift cluster using Tekton
pipelines, not in external CI systems. This keeps the build process close to
the deployment environment and leverages the cluster's internal image registry.

The pipeline is parameterized so it can build any variant's Containerfile
by simply providing different parameters in the PipelineRun.

```text
PipelineRun ──► Pipeline ──► git-clone ──► buildah-build-push ──► Internal Registry
```

### Internal Image Registry

Images are pushed to:

```text
image-registry.openshift-image-registry.svc:5000/<namespace>/<image>:<tag>
```

This avoids external registry dependencies and keeps images within the cluster
where DevSpaces can pull them directly.

### Documentation Pipeline

Documentation is built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)
and deployed to GitHub Pages via the `pages.yml` workflow on every push to
`main`.
