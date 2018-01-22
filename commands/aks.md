# Azure AKS Deployment Commands for Windows 10

### Setup
Install Azure CLI

### Steps before performing any action on Azure portal through CLI
Login to Azure
```
az login
```
_Note: [Set Proxy](#setting-proxy) before running any cli command if inside enterprise network_ 

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
