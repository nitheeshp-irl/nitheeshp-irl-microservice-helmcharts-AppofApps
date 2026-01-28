# Craftista Microservices Deployment

This repository implements a professional, fully automated GitOps deployment architecture using **Argo CD ApplicationSets**. It supports multi-environment scalability (dev, staging, prod) with zero-touch configuration.

## Architecture Highlights

- **Argo CD ApplicationSets**: A single `appset.yaml` controller manages all services across all environments.
- **Matrix Generation**: Automatically discovers environments from the `envs/` directory and deploys the full service stack to each.
- **Multiple Sources**: Combines generic Helm charts with environment-specific values from the same repository.
- **Robust Monitoring**: All services include standardized Kubernetes Liveness and Readiness probes.

## Repository Structure

```bash
├── appset.yaml           # The master controller (ApplicationSet)
├── envs/
│   ├── dev/              # Dev-specific values.yaml files
│   └── staging/          # Staging-specific values.yaml files
├── service-charts/       # Generic, reusable Helm charts for microservices
│   ├── frontend/
│   ├── catalogue/
│   └── ...
└── .gitignore            # Keeps the repo clean (ignoring local temporary files)
```

## Microservices Suite

| Service | Namespace | Purpose |
|---------|-----------|---------|
| **Frontend** | `craftista` | Primary User Interface (Exposed via LoadBalancer) |
| **Catalogue** | `craftista` | Product management API |
| **Catalogue-DB** | `craftista` | PostgreSQL database for catalogue data |
| **Recommendation** | `craftista` | AI-driven product recommendations |
| **Voting** | `craftista` | User ratings and voting platform |

## Getting Started

### 1. Prerequisite
Ensure Argo CD is installed in your cluster and you have the necessary `AppProject` created.

### 2. Deployment
Apply the ApplicationSet to your cluster:
```bash
kubectl apply -f appset.yaml -n argocd
```

### 3. Adding a New Environment
To create a new environment (e.g., `production`):
1. Create a new folder: `mkdir -p envs/production`
2. Copy existing values: `cp envs/dev/*.yaml envs/production/`
3. Commit and Push: `git add . && git commit -m "feat: add production environment"`
4. **Argo CD will automatically detect and deploy 5 new applications** (e.g., `production-frontend`).

## Development & Maintenance

- **Adding a Service**: Add the service name to the `elements` list in `appset.yaml` generators.
- **Updating Charts**: Modify templates in `service-charts/`. These changes propagate to all environments instantly.
- **Fine-tuning**: Adjust `envs/<env>/<service>.yaml` for environment-specific tweaks (replicas, resource limits, etc.).

---
*Built with ❤️ using Antigravity Agentic Coding.*
