# ref-app-docker-web

Create the resource group

`az group create --name yourgroupname --location yourlocation`

Copy resource group id into variable

`groupId = $(az group show --name yourgroupname --query id --output tsv)`

Create the service principal and ***copy output somewhere***

`az ad sp create-for-rbac --scope $groupId --role Contributor --sdk-auth`

Create the container registry (***registry name must be globally unique***)

`az acr create --resource-group yourgroupname --name yourregistryname --sku Basic`

Create the appservice plan

`az appservice plan create --resource-group yourgroupname --name yourplanname`

Create the web app

`az webapp create --resource-group yourgroupname --plan yourplanname --name yourappname`

Add secrets to github settings (refer to the output from `ad az sp` command)

Github secret key | Value to use
--- | ---
AZ_CRED_DEV | entire JSON output
PWD_DEV | clientSecret
REGISTRY_DEV | name of container registry used (i.e. yourregistryname)
RG_DEV | name of resource group used (i.e. yourgroupname)
USER_DEV | clientId




