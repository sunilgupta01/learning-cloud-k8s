# Typical Steps to deploy code on Azure AKS using Azure CLI
_Note: Tested with Azure CLI version **2.0.25** on **Windows 10 64 bit** on **Command Prompt**. All resources created are prefixed by **example**._
## First Time Setup
### Setup
[Install Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

### Steps before performing any action on Azure portal through CLI
Login to Azure
```
az login
```
_Note: [Set Proxy](#setting-proxy) before running any cli command if inside enterprise network_ 

### Create/Delete a Resource Group
```
az group create --name example_rg --location centralus
az group delete -n example_rg
```
_Note: Use a region that supports AKS so that all the resources can be created in same region_

### Create Azure Container Registry, create Service Principal and list the ACR server name
```
az acr create --resource-group rg --name exampleacr --sku Standard
az ad sp create-for-rbac --scopes /subscriptions/<subscription_id>/resourceGroups/example_rg/providers/Microsoft.ContainerRegistry/registries/exampleacr --role Owner --password example_acr_sp_pwd
  Sample Output:
  {
    "appId": "41b1ce21-f813-4203-n3f5-a7n1099ca1fz",
    "displayName": "azure-cli-2018-01-20-17-04-29",
    "name": "http://azure-cli-2018-01-20-17-04-29",,
    "password": "example_acr_sp_pwd",
    "tenant": "3eab25b7-0ae5-47df-a3db-d9c0c81e90b1"
  }
az acr list --resource-group example_rg --query "[].{acrLoginServer:loginServer}" --output table
  Sample Output:
  exampleacr.azurecr.io
```
### Enable AKS preview and Create Azure k8s Cluster
```
az provider register -n Microsoft.ContainerService
az aks create --resource-group example_rg --name example_aks --node-count 1 --generate-ssh-keys
```
### Install kubectl, get aks credentials and verify aks connection
```
az aks install-cli
az aks get-credentials --resource-group example_rg --name example_aks
kubectl get nodes
```
### Create secret to access docker registry by cluster
```
kubectl create secret docker-registry exampleacr_secret --docker-server exampleacr.azurecr.io  --docker-email=userid@domain.com  --docker-username=41b1ce21-f813-4203-n3f5-a7n1099ca1fz --docker-password example_acr_sp_pwd
  It will create a secret for Azure Container Registry. This needs to be mentioned in aks manifest file: imagePullSecrets:
      - name: exampleacr_secret
```
### Create public static IP with DNS name and assigning it to a service
```
az network public-ip create -g MC_example_rg_digexp-example_aks -n example_app_servic_publicip --dns-name example_app --allocation-method Static
  It will result a JSON, which will also have the public IP mentioned. This public IP can be used in service spec in k8s manifest file (spec: type: LoadBalancer | spec: loadBalancerIP: 52.176.148.105) to point application service to a static IP. 
```
### Create storage account and file share to be shared by example application
```
az storage account create -n examplestorage -g example_rg -l centralus --sku Standard_LRS
  Creates storage account
az storage account show-connection-string -n examplestorage -g example_rg -o tsv
  Gets storage account connection string
az storage share create -n examplestorage_fileshare --connection-string DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=examplestorage;AccountKey=A4wajUuLKO8Gzkk82h2cXY3Z2aEHhcK74joZ972wvfwVMnzNUmdO1/ALVhJ7+HuQe+dFXzmypOj5auxhjDwjJA==
  Creates file share
az storage share exists -n examplestorage_fileshare --connection-string DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=examplestorage;AccountKey=A4wajUuLKO8Gzkk82h2cXY3Z2aEHhcK74joZ972wvfwVMnzNUmdO1/ALVhJ7+HuQe+dFXzmypOj5auxhjDwjJA==
  Checks details of shared storage
```
## Subsequent Login
### Login to Azure Container Registry, tag and push docker image to ACR
```
docker login exampleacr.azurecr.io -u 41b1ce21-f813-4203-n3f5-a7n1099ca1fz -p example_acr_sp_pwd
docker tag example_dockerimage exampleacr.azurecr.io/example_dockerimage
docker push exampleacr.azurecr.io/example_dockerimage
```
### Deploy and View k8s dashboard in local
```
kubectl proxy
  It will run k8s dashboard on local with port 8001. Use below URL to access it.
  http://localhost:8001/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard
az aks browse --resource-group example_rg --name example_aks
  It will run k8s dashboard on local with port 8001 and also open the dashboard in browser
```
## Iterative steps
### Deploy a docker image on aks
```
kubectl create secret docker-registry exampleacr_secret --docker-server exampleacr.azurecr.io  --docker-email=userid@domain.com  --docker-username=41b1ce21-f813-4203-n3f5-a7n1099ca1fz --docker-password example_acr_sp_pwd
  It will create a secret for Azure Container Registry. This needs to be mentioned in aks manifest file: imagePullSecrets:
      - name: exampleacr_secret
kubectl create -f example_app.yml
  It will deploy component based on the manifest file - example_app.yml
kubectl get service example_app_service_name --watch
  It will give details of the services, including external IP (if configured)

```

### Setting Proxy
```
@ECHO OFF
set http_proxy=http://<username>:<password>@<proxy_ip>:<proxy_port>
set https_proxy=https://<username>:<password>@<proxy_ip>:<proxy_port>
set HTTP_PROXY=http://<username>:<password>@<proxy_ip>:<proxy_port>
set HTTPS_PROXY=https://<username>:<password>@<proxy_ip>:<proxy_port>
set no_proxy="*.<organization_domain>.com"
@ECHO ON
REM echo proxy settings set
```
