# DevSpaces Vibe Coding Environment

Welcome to the **DevSpaces Vibe Coding Environment** — a curated, Fedora-based
development platform designed for **Red Hat OpenShift DevSpaces**.

## What is this project?

This repository provides ready-to-use development environments (DevFiles) and
their corresponding container images (Containerfiles) for cloud-native Vibe
Coding workflows inside OpenShift DevSpaces.

Container images are built using **OpenShift Pipelines** (Tekton) and pushed
directly to the local OpenShift image registry in the namespace where the
pipeline runs.

## Key Features

- **Fedora-based** — built on a modern, well-maintained Linux distribution.
- **x86_64 architecture** — targeting the supported architecture for Red Hat
  OpenShift.
- **Cloud-native tooling** — `oc`, `kubectl`, `helm`, `kustomize`, `tkn`,
  and more are pre-installed.
- **Multiple DevFile variants** — choose the environment that fits your
  workflow, or create your own.
- **OpenShift Pipelines** — images are built and pushed via Tekton pipelines
  running on the cluster.
- **OpenShift DevSpaces native** — launch a workspace directly from a Git
  repository URL.

## Quick Start

1. Build the base image using OpenShift Pipelines:

    ```bash
    oc apply -f pipelines/pvc.yaml
    oc apply -f pipelines/tasks/
    oc apply -f pipelines/pipeline.yaml
    oc create -f pipelines/runs/build-base.yaml
    ```

2. Open this repository in DevSpaces:

    ```text
    https://<your-devspaces-host>/#https://github.com/devspaces-vibe-coding/devspaces-vibe-coding-env?devfilePath=devfiles/base/devfile.yaml
    ```

See the [Getting Started](getting-started.md) guide for detailed instructions.
