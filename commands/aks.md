# Azure AKS Deployment Commands [Windows]

Install Azure CLI

Login to Azure
az login
Note: set proxy if inside enterprise network 

## Setting Proxy
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
