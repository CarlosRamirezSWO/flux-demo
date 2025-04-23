# flux-demo

This repository is used to demonstrate [FluxCD](https://fluxcd.io/) as a GitOps tool for managing Kubernetes resources.

Flux will continuously monitor this repository and apply the resources defined inside it to your cluster whenever a change is detected.

---

## 🗂 Repository Structure


- `nginx-deployment.yaml`: A basic deployment for an NGINX pod in the `default` namespace.
- `kustomization.yaml`: A Flux-required file that lists the Kubernetes resources to apply from this directory.

---

## 🚀 How This Repository Is Used

1. **This repository is hosted on GitHub.**
2. **Flux is already installed in your Kubernetes cluster and configured to watch this repository.**
3. **Flux automatically applies the manifests in `clusters/my-cluster/` to the cluster.**

---

## 🔁 Flux Integration

To integrate this repository with Flux, apply the following manifest to your cluster:

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: flux-demo
  namespace: flux
spec:
  interval: 1m
  url: https://github.com/CarlosRamirezSWO/flux-demo
  branch: main
  ref:
    branch: main
  secretRef:
    name: flux-system
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: flux-demo
  namespace: flux
spec:
  interval: 1m
  path: ./clusters/my-cluster
  prune: true
  sourceRef:
    kind: GitRepository
    name: flux-demo
