# CargoTrack GitOps

ArgoCD Application manifests and Kubernetes resource definitions for the CargoTrack Logistics Platform.

## Repository Structure

```
cargotrack-gitops/
├── apps/
│   ├── root-app.yaml          # ArgoCD App-of-Apps (bootstrapped by Terraform)
│   └── cargotrack-dev.yaml    # CargoTrack dev environment application
└── k8s/                       # Raw Kubernetes manifests (reference / dry-run)
    ├── namespace.yaml
    ├── configmaps/
    ├── secrets/
    ├── core-service/
    ├── ai-service/
    ├── document-service/
    ├── frontend/
    ├── hpa/
    └── ingress/
```

## ArgoCD Architecture

```
root-app (App-of-Apps)
  └── watches: cargotrack-gitops/apps/
      └── cargotrack-dev
            └── watches: cargotrack-helm/cargotrack/
                  └── deploys to: cargotrack namespace on EKS
```

## Bootstrap

The `root-app` is bootstrapped by Terraform (`kubernetes_manifest.argocd_root_app` in `cargotrack-infra`).
After first apply, ArgoCD manages itself via GitOps.
