# Audit Application Gateway Cipher Suites

This policy audits that an application gateway is set to only use cipher suites found in the suppplied list

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FAppGateway%2Faudit-appgw-ciphersuites%2Fazurepolicy.json)
[![Deploy to Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FAppGateway%2Faudit-appgw-ciphersuites%2Fazurepolicy.json)

## Try with Azure PowerShell

````powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'audit-appgw-ciphersuites' -DisplayName 'Audit Application Gateway Cipher Suites' -description 'This policy audits that an application gateway is set to only use cipher suites found in the suppplied list' -Policy 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/AppGateway/audit-appgw-ciphersuites/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/AppGateway/audit-appgw-ciphersuites/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'audit-appgw-ciphersuites-assignment' -DisplayName 'Audit Application Gateway Cipher Suites Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter .\parametervalues.json
````

## Try with Azure CLI

```cli
# Create the Policy Definition (Subscription scope)
az policy definition create --name "audit-appgw-ciphersuites" --display-name "Audit Application Gateway Cipher Suites" --description "This policy audits that an application gateway is set to only use cipher suites found in the suppplied list" --rules "https://raw.githubusercontent.com/carluu/AzurePolicy/master/AppGateway/audit-appgw-ciphersuites/azurepolicy.rules.json" --params "https://raw.githubusercontent.com/carluu/AzurePolicy/master/AppGateway/audit-appgw-ciphersuites/azurepolicy.parameters.json" --mode All

# Create the Policy Assignment
az policy assignment create --name "audit-appgw-ciphersuites-assignment" --display-name "Audit Application Gateway Cipher Suites" --scope <scope> --policy audit-appgw-ciphersuites --params @.\parametervalues.json
```

## Deployment Notes
Deployments above assume that there is a local file called parametervalues.json containing the values for the parameters