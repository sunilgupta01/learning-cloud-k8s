# Typical Steps to deploy code on Azure AKS using Azure CLI
_Note: All resources created will be prefixed by **example** or **example_**
### Setup
Install Azure CLI

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
az acr list --resource-group example_rg --query "[].{acrLoginServer:loginServer}" --output table
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
