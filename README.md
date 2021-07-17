# az-env
Project for an Azure login script plus settings of env vars.

Usage:
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

running env gives:
- SUBSCRIPTION_ID=2430a957-b64b-4aad-9f52-1b0d61f1098f
- RESOURCE_GROUP=hansanders-dev
- KEYVAULT_NAME=hansanders-dev
- APP_DEVOPS_CLIENTID=64dbf515-beef-4758-a646-d4970fd69dd7
- [[ etc; further output deleted ]]

Built into the script is the option to login to a central container registry. 
For this login to work you must set env vars AZURE_CENTRAL_ACR_NAME and AZURE_CENTRAL_ACR_SUBSCRIPTION_ID.

For example, put this into your ~/.profile:
- export AZURE_CENTRAL_ACR_NAME="launchpad001"
- export AZURE_CENTRAL_ACR_SUBSCRIPTION_ID="600a3ac6-4bc1-4816-a800-18f39157d8b6"

## Requirements

jq
