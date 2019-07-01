# Audit MySQL Diagnostic Settings

This policy audits that diagnostic settings have been enabled on a MySQL server

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FMySQL%2Fauditnotexists-mysql-diaglogs%2Fazurepolicy.json)
[![Deploy to Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FMySQL%2Fauditnotexists-mysql-diaglogs%2Fazurepolicy.json)

## Try with Azure PowerShell

````powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'auditnotexists-mysql-diaglogs' -DisplayName 'Audit MySQL Diagnostic Settings' -description 'This policy audits that diagnostic settings have been enabled on a MySQL server' -Policy 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/MySQL/auditnotexists-mysql-diaglogs/azurepolicy.rules.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'auditnotexists-mysql-diaglogs-assignment' -DisplayName 'Audit MySQL Diagnostic Settings Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition
````

## Try with Azure CLI

```cli
# Create the Policy Definition (Subscription scope)
az policy definition create --name "auditnotexists-mysql-diaglogs" --display-name "Audit MySQL Diagnostic Settings" --description "This policy audits that diagnostic settings have been enabled on a MySQL server" --rules "https://raw.githubusercontent.com/carluu/AzurePolicy/master/MySQL/auditnotexists-mysql-diaglogs/azurepolicy.rules.json" --mode All

# Create the Policy Assignment
az policy assignment create --name "auditnotexists-mysql-diaglogs-assignment" --display-name "Audit MySQL Diagnostic Settings Assignment" --scope <scope> --policy auditnotexists-mysql-diaglogs
```