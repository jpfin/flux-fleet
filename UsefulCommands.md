# Useful Commands

## kubectl
Resources
* https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
* https://kubernetes.io/docs/reference/kubectl/cheatsheet/

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

kubectl describe pods -n vault vault-0

kubectl delete pods -n vault vault-0


# get cluster info
kubectl cluster-info   

# dump pod logs (stdout)
kubectl logs pods -n vault vault-0 

# stream pod logs (stdout)
kubectl logs -f pods -n vault vault-0

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

## Kustomise
```
# To confirm that your patch config file changes are correct before applying to the cluster, you can run
kustomize build development/vault

# format file
 kubectl kustomize cfg fmt file_name 

```

## Helm


```
# get helm status
helm status --namespace=vault vault

```


# Useful resources

```
# Helm chart repo
https://artifacthub.io/

```


## GIT Actions

### Touble shooting

# Getting  Permission denied error

Issue:
```
/home/runner/work/_temp/1a79bda7-744a-43e2-829c-11542d4bc1e0.sh: line 1: ./scripts/validate.sh: Permission denied
Error: Process completed with exit code 126.
```
Resolution:
https://stackoverflow.com/questions/42154912/permission-denied-for-build-sh-file
```
git update-index --add --chmod=+x build.sh
git commit -m 'Make build.sh executable'
git push
```


# Linux commands

```
# upgrade and clean
sudo -- sh -c 'apt-get update; apt-get upgrade -y; apt-get dist-upgrade -y; apt-get autoremove -y; apt-get autoclean -y'

```