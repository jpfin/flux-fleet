# Useful Commands

## kubectl
Resources
* https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

```
# list namespaces
kubectl get namespace

# get pods in certain namespace
kubectl get pods --namespace=vault

# exec into pod in certain namespace
kubectl exec -ti --namespace=consul server-0 -- /bin/bash
kubectl exec -ti --namespace=vault vault-0 -- vault operator init

# override default context
kubectl config set-context --current --namespace=<insert-namespace-name-here>
# Validate override
kubectl config view --minify | grep namespace:

# persistent volumnes
kubectl -n vault get pvc

# Check the location and credentials that kubectl knows about  
kubectl config view

# Change the current context to 'kind-staging'. Useful if need to switch between clusters
kubectl config use-context kind-staging
kubectl config use-context kind-staging-vault

# get pod details
kubectl get pods -n vault vault-0 -o yaml 

kubectl describe pods -n vault vault

# get cluster info
kubectl cluster-info   


```


## flux

```
# 
flux get all
```

## kind
```
# list clusters
kind get clusters

# delete cluster
kind delete cluster --name staging
```

# Useful resources

```
# Helm chart repo
https://artifacthub.io/

```