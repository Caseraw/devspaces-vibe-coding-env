# Base Environment

The **base** variant is the default development environment. It provides a
Fedora 42 image (x86_64) loaded with cloud-native CLI tools and general-purpose
development utilities.

## Container Image

Built via OpenShift Pipelines and pushed to the internal registry:

```text
image-registry.openshift-image-registry.svc:5000/<namespace>/devspaces-vibe-coding-base:latest
```

### Installed Tools

#### Cloud-Native & Kubernetes

| Tool | Description |
|---|---|
| `oc` | OpenShift CLI |
| `kubectl` | Kubernetes CLI |
| `helm` | Kubernetes package manager |
| `kustomize` | Kubernetes configuration management |
| `tkn` | Tekton Pipelines CLI |
| `kn` | Knative CLI |
| `virtctl` | KubeVirt CLI |
| `roxctl` | Red Hat Advanced Cluster Security CLI |

#### Cloud Providers

| Tool | Description |
|---|---|
| `aws` | AWS CLI v2 |
| `az` | Azure CLI |
| `rosa` | Red Hat OpenShift on AWS CLI |
| `openshift-install` | OpenShift Installer |

#### Container Tools

| Tool | Description |
|---|---|
| `podman` | Daemonless container engine |
| `buildah` | OCI image builder |
| `skopeo` | Container image operations |

#### General Development

| Tool | Description |
|---|---|
| `git` | Version control |
| `python3` / `pip` | Python runtime |
| `make` / `gcc` / `gdb` | Build and debug toolchain |
| `ansible` | Automation platform |
| `jq` | JSON processor |
| `vim` / `nano` | Text editors |
| `tmux` / `screen` | Terminal multiplexers |

#### Networking & Diagnostics

| Tool | Description |
|---|---|
| `curl` / `wget` | HTTP clients |
| `nmap` | Network scanner |
| `tcpdump` | Packet analyzer |
| `mtr` / `traceroute` | Network path analysis |
| `iperf3` | Bandwidth measurement |
| `htop` | Process monitor |

## DevFile

The base devfile requests the following workspace resources:

| Resource | Value |
|---|---|
| Memory limit | 4 Gi |

## Building

See [OpenShift Pipelines](../pipelines.md) for build instructions.

## Source Files

- Containerfile: `devfiles/base/Containerfile`
- DevFile: `devfiles/base/devfile.yaml`
- PipelineRun: `pipelines/runs/build-base.yaml`
