# az-env scripts

Project for an Azure login script (az-env) including settings of env vars, and a script to build and execute Pentaho DI Docker containers both locally and as deployment group (az-drun). The project is named after the script that started it all, hence the duplication of title and script.
These scripts assume our standards and best practices, so may require adaptation to your specific environment.

## az-env

Script that logs you in into Azure and sets a lot of env vars.

Installation:

- put az-env in your ~/bin directory
- create aliases for whatever environments you have in your subscription in your shell profile

Examples:

- alias az-env-dev='~/bin/az-env dev && source ~/.az-env-vars'
- alias az-env-tst='~/bin/az-env tst && source ~/.az-env-vars'

Usage:
az-env [ \<environment> ]

environment: *dev/tst/acc/prd*

- run the alias of your choice
- result:
  - you are logged into the subscription you selected and
  - your subcription ID, resource group and keyvault name are available as env vars
  - all secrets are available in your current shell as env vars

The secrets are converted by their name in the sense that dashes are replaced by underscores, and the names are made all uppercase.
Setting the env vars based on secrets is a rather slow process. A dotted bar is shown to indicate progress.
Unless you have Python installed, then a far more quicker method is being used with a small inline Python script.

Execute:

- az-env-dev

This will prompt an Azure login screen where you select your account to be used. After this, you need to select the subscription to use. The last option is 'Quit' in case you want to stop there.

After this, running *env* gives:

- SUBSCRIPTION_ID=2434a957-464b-3a43-9f33-1b0d41f1098f
- RESOURCE_GROUP=client-dev
- KEY_VAULT_NAME=client-dev
- APP_DEVOPS_CLIENTID=64db3515-b3ef-4353-a343-d4373fd693d7
- [[ etc; further output deleted ]]

Built into the script is the option to login to a central container registry.
For this login to work you must set env vars AZURE_CENTRAL_ACR_NAME and AZURE_CENTRAL_ACR_SUBSCRIPTION_ID.

For example, put this into your ~/.profile (**this is NOT the real SUBSCRIPTION_ID to prevent information leakage, substitute with the real one**):

- export AZURE_CENTRAL_ACR_NAME="launchpad001"
- export AZURE_CENTRAL_ACR_SUBSCRIPTION_ID="602a3ac6-####-####-####-18f39152d8b6"

## az-drun

Installation:

- put az-drun in your ~/bin directory

Usage:
az-drun [ \<environment> [ \<container deployment parameters> [ \<container base name> ] ] ]

environment: *dev/tst/acc/prd/local*, where *local* will run the Docker container locally

container base name: default of *dwh-etl* will be resulting in container names such as client-dwh-etl and images names such as client-dwh-etl:rkooijman

The script that is run in the container is /etl/scripts/etl_${USER}.sh because we are developing here. Do you want to run the standard daily job or what have you? Just symlink /etl/scripts/etl_${USER}.sh --> /etl/scripts/dwh_daily.sh for example.

## acr-login

Installation:

- put acr-login in your ~/bin directory

Usage:
acr-login

This script just does a login for you into the ACR using the env vars set earlier by az-env.

## az-liquibase

Installation:

- put az-liquibase in your ~/bin directory

Usage:
az-liquibase [ \<environment> [ \<container base name> ] ]

This script will apply `master.xml` in your *changelogs* directory to your Snowflake database.
This script is highly dependent on our way of working with Snowflake database, so it will need tailoring for your environment and/or database.

## Requirements

- bash version 4+ (should suffice, I run version 5.1 myself)
- jq, <https://stedolan.github.io/jq/>
- az cli tools, <https://docs.microsoft.com/en-us/cli/azure/install-azure-cli>

If you want to take advantage of faster retrieval of secrets through Python:

- Python 3.7+
- Python packages: azure-identity azure-keyvault

## TODO

- optimize logins: check for any logged in accounts that are still valid for some time
