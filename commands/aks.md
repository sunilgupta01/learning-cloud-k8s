# Typical Steps to deploy code on Azure AKS using Azure CLI
_Note: All resources created will be prefixed by **example**_
### Setup
[Install Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

### Steps before performing any action on Azure portal through CLI
Login to Azure
```
az login
```
_Note: [Set Proxy](#setting-proxy) before running any cli command if inside enterprise network_ 

### Create a Resource Group
```
az group create --name example_rg --location centralus
```
_Note: Use a region that supports AKS so that all the resources can be created in same region_

### Create Azure Container Registry, create Service Principal and list the ACR server name
```
az acr create --resource-group rg --name exampleacr --sku Standard
az ad sp create-for-rbac --scopes /subscriptions/<subscription_id>/resourceGroups/example_rg/providers/Microsoft.ContainerRegistry/registries/exampleacr --role Owner --password example_acr_sp_pwd
  Sample Output: Azure AD Service Principal: {"appId":  "41b1ce21-f813-4203-n3f5-a7n1099ca1fz", "displayName": "azure-cli-2018-01-20-17-04-29", "name": "http://azure-cli-2018-01-20-17-04-29", "password": "example_acr_sp_pwd", "tenant": "3eab25b7-0ae5-47df-a3db-d9c0c81e90b1"}
az acr list --resource-group example_rg --query "[].{acrLoginServer:loginServer}" --output table
  Sample Output: exampleacr.azurecr.io
```
### Login to Azure Container Registry, tag and push docker image to ACR
```
docker login exampleacr.azurecr.io -u 41b1ce21-f813-4203-n3f5-a7n1099ca1fz -p example_acr_sp_pwd
docker tag example_dockerimage exampleacr.azurecr.io/example_dockerimage
docker push exampleacr.azurecr.io/example_dockerimage
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
### Deploy and View k8s dashboard in local
```
kubectl proxy
  It will run k8s dashboard on local with port 8001. Use below URL to access it.
		http://localhost:8001/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard
az aks browse --resource-group example_rg --name example_aks
	It will run k8s dashboard on local with port 8001 and also open the dashboard in browser
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
