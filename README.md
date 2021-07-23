# az-env
Project for an Azure login script including settings of env vars, and script to build and execute Pentaho DI Docker containers both locally and as ACI. The project is named after the script that started it all, hence the duplication of title and script.

## az-env
Installation and usage:
- put az-env in your ~/bin directory
- create aliases for whatever environments you have in your subscription in your shell profile
- run the alias of your choice
- result: 
    - you are logged into the subscription you selected and 
    - your subcription ID, resource group and keyvault name are available as env vars
    - all secrets are available in your current shell as env vars

The secrets are converted by their name in the sense that dashes are replaced by underscores, and the names are made all uppercase.

Examples:
- alias az-env-dev='~/bin/az-env dev && source ~/.az-env-vars'
- alias az-env-tst='~/bin/az-env tst && source ~/.az-env-vars'

Execute:
- az-env-tst

running *env* gives:
- SUBSCRIPTION_ID=2430a957-b64b-4aad-9f52-1b0d61f1098f
- RESOURCE_GROUP=hansanders-dev
- KEY_VAULT_NAME=hansanders-dev
- APP_DEVOPS_CLIENTID=64dbf515-beef-4758-a646-d4970fd69dd7
- [[ etc; further output deleted ]]

Built into the script is the option to login to a central container registry. 
For this login to work you must set env vars AZURE_CENTRAL_ACR_NAME and AZURE_CENTRAL_ACR_SUBSCRIPTION_ID.

For example, put this into your ~/.profile:
- export AZURE_CENTRAL_ACR_NAME="launchpad001"
- export AZURE_CENTRAL_ACR_SUBSCRIPTION_ID="600a3ac6-4bc1-4816-a800-18f39157d8b6"

## az-drun
Installation:
- put az-drun in your ~/bin directory

Usage:
az-drun [ \<environment> [ \<container base name> ] ]

environment: *dev/tst/acc/prd/local*, where *local* will run the Docker container locally

container base name: default *dwh-etl* will be resulting in container names such as client-dwh-etl and images names such as client-dwh-etl:rkooijman


## Requirements

jq, https://stedolan.github.io/jq/

