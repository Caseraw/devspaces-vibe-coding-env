# OpenShift Pipelines

Container images for DevFile variants are built using **OpenShift Pipelines**
(Tekton) and pushed to the local OpenShift image registry.

## Architecture

```text
PipelineRun (runs/build-base.yaml)
  │
  └── Pipeline (pipeline.yaml)
        │
        ├── Task: git-clone ──► Clone repo into shared workspace
        │
        └── Task: buildah-build-push ──► Build image and push to internal registry
              │
              └── image-registry.openshift-image-registry.svc:5000/<namespace>/<image>:<tag>
```

## Resources

| File | Kind | Description |
|---|---|---|
| `pipelines/tasks/git-clone.yaml` | Task | Clones a Git repository |
| `pipelines/tasks/buildah.yaml` | Task | Builds and pushes a container image |
| `pipelines/pipeline.yaml` | Pipeline | Chains clone → build → push |
| `pipelines/pvc.yaml` | PersistentVolumeClaim | Shared workspace between tasks |
| `pipelines/runs/build-base.yaml` | PipelineRun | Builds the base image |

## Pipeline Parameters

The `build-devfile-image` pipeline accepts the following parameters:

| Parameter | Default | Description |
|---|---|---|
| `git-url` | — | Git repository URL to clone |
| `git-revision` | `main` | Branch, tag, or commit SHA |
| `image-name` | — | Full internal registry image reference |
| `containerfile-path` | `devfiles/base/Containerfile` | Path to the Containerfile |
| `build-context` | `.` | Build context directory |

## Usage

### First-time setup

Apply the shared resources (PVC, Tasks, Pipeline) once per namespace:

```bash
oc apply -f pipelines/pvc.yaml
oc apply -f pipelines/tasks/
oc apply -f pipelines/pipeline.yaml
```

### Build the base image

Edit `pipelines/runs/build-base.yaml` and replace `<namespace>` with your
actual OpenShift namespace, then create the PipelineRun:

```bash
oc create -f pipelines/runs/build-base.yaml
```

### Monitor a PipelineRun

```bash
tkn pipelinerun logs -f -L
```

Or list all runs:

```bash
tkn pipelinerun list
```

### Build a different variant

Create a new PipelineRun YAML under `pipelines/runs/` with the appropriate
`containerfile-path` and `image-name` parameters. For example, to build a
hypothetical `python` variant:

```yaml
apiVersion: tekton.dev/v1
kind: PipelineRun
metadata:
  generateName: build-python-
spec:
  pipelineRef:
    name: build-devfile-image
  params:
    - name: git-url
      value: https://github.com/devspaces-vibe-coding/devspaces-vibe-coding-env.git
    - name: git-revision
      value: main
    - name: image-name
      value: image-registry.openshift-image-registry.svc:5000/my-namespace/devspaces-vibe-coding-python:latest
    - name: containerfile-path
      value: devfiles/python/Containerfile
    - name: build-context
      value: "."
  workspaces:
    - name: shared-workspace
      persistentVolumeClaim:
        claimName: devspaces-vibe-coding-ws
```

## Troubleshooting

**Pipeline SA missing image push permissions**

The `pipeline` ServiceAccount (created automatically by the OpenShift Pipelines
operator) needs the `system:image-builder` role to push to the internal
registry:

```bash
oc policy add-role-to-user system:image-builder system:serviceaccount:$(oc project -q):pipeline
```

**Build fails with storage errors**

The default storage driver is `vfs`, which works without elevated privileges
but uses more disk space. If the PVC runs out of space, increase the storage
request in `pipelines/pvc.yaml`.
