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
kubectl exec -ti --namespace=vault vault-0 -- vault operator init

# override default context
kubectl config set-context --current --namespace=<insert-namespace-name-here>

# Validate override
kubectl config view --minify | grep namespace:
```


## flux

```
# 
flux get all
```

## kind

