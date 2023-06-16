# ref-app-docker-web

Create the resource group

`az group create --name yourgroupname --location yourlocation`

Copy resource group id into variable

`groupId = $(az group show --name yourgroupname --query id --output tsv)`

Create the service principal and ***copy output somewhere***

`az ad sp create-for-rbac --scope $groupId --role Contributor --sdk-auth`

Create the container registry (***registry name must be globally unique***)

`az acr create --resource-group yourgroupname --name yourregistryname --sku Basic`



