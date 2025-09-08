# Microservices Manifests

This directory contains centralized Kustomize configurations and ArgoCD Application manifests organized by environment.

## Structure

```
manifests-microservices/
├── kustomize/                      # Centralized Kustomize configurations
│   ├── api-service/
│   │   ├── base/                   # Base Kubernetes resources
│   │   └── overlays/
│   │       ├── production/         # Production environment overlay
│   │       ├── staging/            # Staging environment overlay
│   │       └── integration-001/    # Integration environment overlay
│   └── store-service/
│       ├── base/                   # Base Kubernetes resources
│       └── overlays/
│           ├── production/         # Production environment overlay
│           ├── staging/            # Staging environment overlay
│           └── integration-001/    # Integration environment overlay
├── applications/                   # ArgoCD Application manifests
│   ├── production/                 # Production environment applications
│   │   ├── api-service-production.yaml # API service deployment
│   │   └── store-service-production.yaml # Store service deployment
│   ├── staging/                    # Staging environment applications
│   │   ├── api-service-staging.yaml # API service deployment
│   │   └── store-service-staging.yaml # Store service deployment
│   └── integration-001/            # Integration environment applications
│       ├── api-service-integration-001.yaml # API service deployment
│       ├── store-service-integration-001.yaml # Store service deployment
│       ├── applicationset-api-service-pr.yaml # API service PR canaries
│       ├── applicationset-api-service-edge-pr.yaml # API service PR routing
│       ├── applicationset-store-service-pr.yaml # Store service PR canaries
│       └── applicationset-store-service-routing-pr.yaml # Store service PR routing
└── README.md                       # This file
```

## Environments

- **production** - Production environment (no canaries)
- **staging** - Pre-production testing environment (no canaries)
- **integration-001** - Integration testing environment with PR canaries

## Adding a New Service

1. Create Kustomize configuration in `kustomize/your-service/`:
   - `base/` - Base Kubernetes resources (Deployment, Service, etc.)
   - `overlays/production/` - Production environment overlay
   - `overlays/staging/` - Staging environment overlay
   - `overlays/integration-001/` - Integration environment overlay
2. Add ArgoCD applications to each environment folder:
   - `applications/production/your-service-production.yaml` - Production deployment
   - `applications/staging/your-service-staging.yaml` - Staging deployment
   - `applications/integration-001/your-service-integration-001.yaml` - Integration environment deployment
3. Add PR canary ApplicationSets to `applications/integration-001/`:
   - `applicationset-your-service-pr.yaml` - PR canary deployments
   - `applicationset-your-service-edge-pr.yaml` - PR edge routing
   - `applicationset-your-service-routing-pr.yaml` - PR service routing

## Adding a New Environment

1. Create Kustomize overlays for each service:
   - `kustomize/api-service/overlays/your-environment/`
   - `kustomize/store-service/overlays/your-environment/`
   - Add more services as needed
2. Create ArgoCD applications directory: `manifests-microservices/applications/your-environment/`
3. Add service applications:
   - `applications/your-environment/api-service-your-environment.yaml`
   - `applications/your-environment/store-service-your-environment.yaml`
   - Add more services as needed

## File Naming Convention

- `{service}-{environment}.yaml` - Service deployment for the environment
- `applicationset-{service}-pr.yaml` - PR canary deployments
- `applicationset-{service}-routing-pr.yaml` - PR routing configurations
- `applicationset-{service}-edge-pr.yaml` - PR edge configurations

## Benefits of Centralized Kustomize

- **Single Source of Truth**: All deployment configurations in one repository
- **Environment Management**: Add new environments in one place
- **Automated Updates**: GitHub Actions can update Kustomize when services are released
- **Consistency**: All services follow the same deployment patterns
- **Maintainability**: Easier to update common configurations across all services

## ArgoCD Integration

The localdev ArgoCD application (`localdev/apps/argocd-applications.yaml`) references this directory to deploy all service applications.
