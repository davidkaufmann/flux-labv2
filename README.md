# Sample Flux Repository

> **Summary:** This repository is a **minimal example repo** for **training purposes** around GitOps with **Flux** used throughout the SVA Academy course `GitOps with FLux`. It serves as a starting point for labs where participants will **incrementally extend** the setup.

## 🎯 Purpose of the Repository
- Demonstrates the **core idea of GitOps**: Declarative state stored in Git, reconciled into the cluster by Flux.
- Provides a **minimal, easy-to-understand baseline** for exercises.
- Will be **expanded step by step** during labs (workloads, policies, observability, multi-tenancy, etc.).

## 🏗 Based on Standard Flux Bootstrap
- Structure follows the **Flux standard bootstrap**
- Clear separation of **cluster base** (CRDs/controllers), **infrastructure** (namespaces, services), and **apps** (deployments/HelmReleases).
- Paths and Kustomize layers are designed for **readability and extensibility**.

## 🔖 Flux Version
- This repo references **Flux v2.6.4**, see `clusters/flux-system/gotk-components.yaml`
- **Do not upgrade** unless explicitly instructed in a lab step.

## 📂 Repository Structure (minimal)
```
.
├─ apps/                        # Example workloads
├─ clusters/                    # Usually contains multiple clusters
├─ infrastructure/              # Base infra (namespaces, repos, services)
└─ README.md
```

## 🚀 Quickstart in case you want to try it out without a lab.
Although i believe it's rather pointless.
1. **Requirements**: Kubernetes cluster, `kubectl`, `flux` CLI (v2.6.4), Git access.
2. **Fork this repo**: Ideally to your user space. Have a `GITHUB_TOKEN` in place.
3. **Bootstrap (example)**:
   ```bash
   flux bootstrap git --url=https://github.com/${GITHUB_USER}/flux-lab --username=${GITHUB_USER} --password=${GITHUB_TOKEN} --token-auth=true --path=clusters --components-extra=image-reflector-controller,image-automation-controller --silent
   ```
4. **Check status**:
   ```bash
   flux get all
   ```

## 🔍 Useful Commands
```bash
# Show sources
flux get sources git -A
flux get sources helm -A

# Trigger sourcecode reconciliation
flux reconcile source git flöux-system -n flux-system

# Check events/logs
kubectl -n flux-system logs deploy/source-controller
kubectl -n flux-system logs deploy/kustomize-controller
kubectl -n flux-system logs deploy/helm-controller
```

---

**Note:**
For multi-cluster setups, mirror the structure under `clusters/<cluster-name>/` and keep kustomizations separated per cluster.
Obviously bootstrapping would have to reference the appropriate path.
`--path=clusters` would update to `--path=clusters/<cluster-name>`
