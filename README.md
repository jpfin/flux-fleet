# flux-fleet


## Repository structure

Based on example repo https://github.com/fluxcd/flux2-kustomize-helm-example

The Git repository contains the following top directories:

### High Level structure

- **apps** dir contains Helm releases with a custom configuration per cluster
- **infrastructure** dir contains common infra tools such as NGINX ingress controller and Helm repository definitions
- **clusters** dir contains the Flux configuration per cluster

```
├── apps
│   ├── base
│   ├── production 
│   └── staging
├── infrastructure
│   ├── nginx
│   ├── redis
│   └── sources
└── clusters
    ├── production
    └── staging 

```

### Application structure

The apps configuration is structured into:

- **apps/base/** dir contains namespaces and Helm release definitions
- **apps/production/** dir contains the production Helm release values
- **apps/staging/** dir contains the staging values

```
./apps/
├── base
│   └── <my app>
│       ├── kustomization.yaml
│       ├── namespace.yaml
│       └── release.yaml
├── production
│   ├── kustomization.yaml
│   └── <my app>-patch.yaml
└── staging
    ├── kustomization.yaml
    └── <my app>-patch.yaml
```

### Infrastructure structure

```
./infrastructure/
├── nginx
│   ├── kustomization.yaml
│   ├── namespace.yaml
│   └── release.yaml
├── redis
│   ├── kustomization.yaml
│   ├── namespace.yaml
│   └── release.yaml
└── sources
    ├── bitnami.yaml
    ├── kustomization.yaml
    └── <my app>.yaml
```

### Cluster structure

The clusters dir contains the Flux configuration:

```
./clusters/
├── production                 
│   ├── flux-system             <-- Boot strapped flux folder
│   ├── apps.yaml               <-- Added. Tell flux where to monitor apps for prod 
│   └── infrastructure.yaml     <-- Added. Tell flux where to monitor apps for prod
└── staging                    
│   ├── flux-system             <-- Boot strapped flux folder
│   ├── apps.yaml               <-- Added. Tell flux where to monitor apps for prod
│   └── infrastructure.yaml     <-- Added. Tell flux where to monitor apps for prod

```
#### Flux Repository structure notes

* Flux monitors ./clusters/<cluster> by default
* ./clusters/<cluster>/apps.yaml points to apps configuration for cluster. eg.  ./clusters/staging/apps.yaml --> ./apps/staging
* ./clusters/<cluster>/infrastructure.yaml points to infra configuration for cluster, eg.  ./clusters/staging/infrastructure.yaml --> ./infrastructure/staging
* kustimisazation files are read first 