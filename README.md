# ref-app-docker-web

Create the resource group

`az group create --name yourgroupname --location yourlocation`

Copy resource group id into variable

`groupId = $(az group show --name yourgroupname --query id --output tsv)`

Create the service principal and ***copy output somewhere***

`az ad sp create-for-rbac --scope $groupId --role Contributor --sdk-auth`

Create the container registry (***registry name must be globally unique***)

`az acr create --resource-group yourgroupname --name yourregistryname --sku Basic --admin-enabled true`

Add secrets to github settings (refer to the output from `ad az sp` command)

Github secret key | Value to use
--- | ---
AZ_CRED_DEV | entire JSON output
PWD_DEV | clientSecret
REGISTRY_DEV | login server of contain registry (i.e. yourregistryname.azurecr.ioc)
RG_DEV | name of resource group used (i.e. yourgroupname)
USER_DEV | clientId

Ensure the variable for `INIT_RUN` is set to true and run the pipeline to create the registry and push the build.

~Create the appservice plan~

`az appservice plan create --resource-group yourgroupname --name yourplanname`

~Create the web app~

`az webapp create --resource-group yourgroupname --plan yourplanname --name yourappname`

*We will create the appservice plan and webapp in the portal for now as the above commands are incorrect*

1. In the portal, create a new app service.
2. Deploy it to the resource group created above.
3. Use `launchwebdev` for the webapp name or edit the `deploy-to-azure.yml` file.
4. Select `Docker Container` for Publish and `Linux` for Operating System.
5. Create new app service plan with appropriate name and SKU.
6. Click "Next: Docker" button and continue setup.
7. Select "Azure Container Registry" for image source
8. Select the appropriate values for registry, image, and tag.
9. Click "Review + create"

Validate that you can load the site.

Change the variable for `INIT_RUN` is not set to true.

Make change in the web app, push code, and execute pipeline.







