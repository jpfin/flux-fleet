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


# Application Notes

Verify install with command
```
kubectl get pods --all-namespaces
```
## Vault 

### UI

To view UI, port foward with ```kubectl port-forward vault-0 8200:8200 ``` Can maybe apply this as part of value/config. Need to look into this more


### Sealed vault

Vault will start as not ready, as it will be 'sealed'. Need to unseal following https://www.vaultproject.io/docs/platform/k8s/helm/run (snippet below). In future plan to provide a rest service/mechanism to automate this.

```
vault-0                                 0/1     Running   0          1m49s
```

```
kubectl exec -ti --namespace=vault vault-0 -- vault operator init
Unseal Key 1: MBFSDepD9E6whREc6Dj+k3pMaKJ6cCnCUWcySJQymObb
Unseal Key 2: zQj4v22k9ixegS+94HJwmIaWLBL3nZHe1i+b/wHz25fr
Unseal Key 3: 7dbPPeeGGW3SmeBFFo04peCKkXFuuyKc8b2DuntA4VU5
Unseal Key 4: tLt+ME7Z7hYUATfWnuQdfCEgnKA2L173dptAwfmenCdf
Unseal Key 5: vYt9bxLr0+OzJ8m7c7cNMFj7nvdLljj0xWRbpLezFAI9

Initial Root Token: s.zJNwZlRrqISjyBHFMiEca6GF
##...

## Unseal the first vault server until it reaches the key threshold, 3 of the 5 generated keys
$ kubectl exec -ti --namespace=vault vault-0 -- vault operator unseal MBFSDepD9E6whREc6Dj+k3pMaKJ6cCnCUWcySJQymOb
$ kubectl exec -ti --namespace=vault vault-0 -- vault operator unseal zQj4v22k9ixegS+94HJwmIaWLBL3nZHe1i+b/wHz25fr
$ kubectl exec -ti --namespace=vault vault-0 -- vault operator unseal 7dbPPeeGGW3SmeBFFo04peCKkXFuuyKc8b2DuntA4VU5

## Verify now ready
kubectl get pods --all-namespaces
or
kubectl get pods --namespace=vault

```