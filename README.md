# azure-k8s-couchbase-cloud-shell
Deploy Couchbase cluster in Azure K8s cluster via cloud shell

Steps to create Azure side resources (Resource Group and Azure kubernetes cluster) and Couchbase(CB) side resources (CB Operator, CB Cluster). Finally we cleanup the resources created.

- [x] Highly recommended to **download/clone** the repo into local directory.

## Create resource group
```az group create --name [GroupName] --location eastus```
(Service is currently available in “eastus”)

## Create AKS cluster
```az aks create --resource-group [GroupName] --name [AKSName] --node-count 1 --generate-ssh```
 
## Get AKS credentials
```az aks get-credentials --resource-group=[GroupName]  --name=[AKSName]```
 
## Test credentials 
```kubectl cluster-info```
 
## Get nodes
```kubectl get nodes```

# Edit the operator and couchbase-cluster.yaml file, change following lines:
  - file: operator.yaml
  ```name: couchbase-operator-CUSTOMER_NAME => name: couchbase-operator-mycustomer```
  
  Similar change in file couchbase-cluster.yaml
  - file: couchbase-cluster.yaml
  ```name:cb-example-aks-CUSTOMER_NAME => name: cb-example-aks-mycustomer```
  
# (Optional) Edit the bucket name to something appropriate
  - file: couchbase-cluster.yaml
  ```name: AzResourceGroupBucket => name: mytestbucket```
  
# Deploy Couchbase operator and cluster
```kubectl create -f secret.yaml```
```kubectl create -f operator.yaml```
```kubectl create -f couchbase-cluster.yaml```

## Expose Couchbase UI
```kubectl get service```
```kubectl edit service cb-example-ui```

Change ```“type: NodePort”``` -> ```“type: LoadBalancer” [its an VI editor]```

```kubectl get service --watch``` [Wait for external ip to be provisioned]

… Open Couchbase console, Add steps to show self healing scenario, etc …
Delete K8S deployment, service and pods

# Cleanup
Delete deployment first, deleting pods will not help as k8s will spin up a lost pod, as it will think this being a service failure. Go to last step if you want to delete in one command

```kubectl delete deployment [deployment-name]```
```kubectl delete service [service-name]```
```kubectl delete pods -l app=cb-example``` or ```kubectl delete pods --all``` [CAUTION: It might delete your ALL other pods too]

# Delete K8S cluster
```az aks delete --name <aks-k8s-cluster-name> --resource-group <resource-gp-name>```
Above command can take anywhere between 2-5 mins, may be more.

## Get Group List
```az group list --output table```

## Delete resource group (Brute Force) to delete everything in one GO
```az group delete --name [GroupName]```
