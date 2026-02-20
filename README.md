# DevSpaces Vibe Coding Environment

A curated, Fedora-based development platform for **Red Hat OpenShift DevSpaces**.

This repository provides ready-to-use development environments (DevFiles) and
their corresponding container images (Containerfiles) for cloud-native Vibe
Coding workflows. Container images are built with **OpenShift Pipelines**
(Tekton) and pushed to the local OpenShift image registry.

## Quick Start

Open this repository in OpenShift DevSpaces, specifying the devfile path:

```text
https://<your-devspaces-host>/#https://github.com/devspaces-vibe-coding/devspaces-vibe-coding-env?devfilePath=devfiles/base/devfile.yaml
```

## What's Included

The **base** environment ships with:

- **Kubernetes & OpenShift** — `oc`, `kubectl`, `helm`, `kustomize`, `tkn`, `kn`, `virtctl`
- **Security** — `roxctl` (Red Hat ACS)
- **Cloud CLIs** — `aws`, `az`, `rosa`, `openshift-install`
- **Container tools** — `podman`, `buildah`, `skopeo`
- **Development** — `git`, `python3`, `make`, `gcc`, `ansible`, `jq`
- **Networking** — `curl`, `nmap`, `tcpdump`, `mtr`, `iperf3`

## Project Structure

```
├── devfiles/                   DevFile variants and container images
│   └── base/
│       ├── Containerfile       Fedora-based container image (x86_64)
│       └── devfile.yaml        DevSpaces workspace definition
├── pipelines/                  OpenShift Pipelines (Tekton) resources
│   ├── tasks/
│   │   ├── git-clone.yaml      Task: clone a Git repository
│   │   └── buildah.yaml        Task: build and push with buildah
│   ├── pipeline.yaml           Pipeline: build-devfile-image
│   ├── pvc.yaml                Shared workspace PVC
│   └── runs/
│       └── build-base.yaml     PipelineRun for the base image
├── docs/                       Documentation source (MkDocs)
├── .github/workflows/          GitHub Pages deployment
├── .devcontainer/              VS Code / GitHub Codespaces support
└── mkdocs.yml                  Documentation configuration
```

## Building Images with OpenShift Pipelines

Apply the pipeline resources and trigger a build:

```bash
oc apply -f pipelines/pvc.yaml
oc apply -f pipelines/tasks/
oc apply -f pipelines/pipeline.yaml
oc create -f pipelines/runs/build-base.yaml
```

See the [Pipelines documentation](https://devspaces-vibe-coding.github.io/devspaces-vibe-coding-env/pipelines/) for details.

## Available DevFile Variants

| Variant | Description | Image |
|---|---|---|
| `base` | Fedora 42 + cloud-native CLI tools | `<internal-registry>/<namespace>/devspaces-vibe-coding-base:latest` |

## Documentation

Full documentation is available at:
**[https://devspaces-vibe-coding.github.io/devspaces-vibe-coding-env/](https://devspaces-vibe-coding.github.io/devspaces-vibe-coding-env/)**

## Contributing

See [CONTRIBUTING](https://devspaces-vibe-coding.github.io/devspaces-vibe-coding-env/contributing/) for guidelines on adding new DevFile variants and submitting changes.

## License

[Apache License 2.0](LICENSE)
