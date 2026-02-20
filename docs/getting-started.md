# Getting Started

This guide walks you through building the base container image with OpenShift
Pipelines and launching your first Vibe Coding workspace in DevSpaces.

## Prerequisites

| Requirement | Details |
|---|---|
| OpenShift cluster | 4.12+ with **OpenShift Pipelines** and **DevSpaces** operators installed |
| `oc` CLI | Logged in to the cluster with permissions to create pipelines |
| Namespace | A namespace/project where pipelines will run and images will be stored |
| Browser | Chrome, Firefox, or Edge (latest) |

## Step 1 — Build the base image

First, edit the PipelineRun to set your namespace:

```bash
# Replace <namespace> with your actual OpenShift namespace
sed 's/<namespace>/my-namespace/g' pipelines/runs/build-base.yaml > /tmp/run.yaml
```

Apply the pipeline resources and trigger the build:

```bash
oc apply -f pipelines/pvc.yaml
oc apply -f pipelines/tasks/
oc apply -f pipelines/pipeline.yaml
oc create -f /tmp/run.yaml
```

Monitor the pipeline run:

```bash
tkn pipelinerun logs -f -L
```

When the pipeline completes, the image will be available in the internal
registry at:

```text
image-registry.openshift-image-registry.svc:5000/<namespace>/devspaces-vibe-coding-base:latest
```

## Step 2 — Launch a workspace

Navigate to your DevSpaces dashboard and create a workspace from the
repository URL, specifying the devfile path:

```text
https://<your-devspaces-host>/#https://github.com/devspaces-vibe-coding/devspaces-vibe-coding-env?devfilePath=devfiles/base/devfile.yaml
```

!!! note
    The devfile references the image from the internal registry. Make sure the
    image was built in the same namespace where DevSpaces can pull from, or
    configure appropriate image pull access.

## Step 3 — Verify your environment

Once the workspace starts, open a terminal and run:

```bash
oc version --client
kubectl version --client
helm version --short
```

All tools listed in the [Base Environment](devfiles/base.md) documentation
should be available.

## Next steps

- Browse the [available DevFile variants](devfiles/index.md)
- Learn about the [pipeline architecture](pipelines.md)
- Read the [project architecture](architecture.md)
- Learn how to [contribute](contributing.md) a new environment
