# Deploy MySQL diagnostic settings if not enabled and pointing to the correct event hub

This policy will deploy MySQL diagnostic settings where the resource does not have them enabled and pointing to the specified event hub.  

It will deploy all available log types (MySqlSlowLogs,MySqlAuditLogs) as well as AllMetrics

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FMySQL%2Fdeploynotexists-mysql-diaglogs-eventhub%2Fazurepolicy.json)
[![Deploy to Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FMySQL%2Fdeploynotexists-mysql-diaglogs-eventhub%2Fazurepolicy.json)

## Try with Azure PowerShell

````powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'deploynotexists-mysql-diaglogs-eventhub' -DisplayName 'Deploy MySQL Diagnostic Settings with Event Hub' -description 'This policy will deploy MySQL diagnostic settings where the resource does not have them enabled and pointing to the specified event hub' -Policy 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/MySQL/deploynotexists-mysql-diaglogs-eventhub/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/MySQL/deploynotexists-mysql-diaglogs-eventhub/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'deploynotexists-mysql-diaglogs-eventhub-assignment' -DisplayName 'Deploy MySQL Diagnostic Settings with Event Hub Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter .\parametervalues.json -AssignIdentity -Location  $scope.Location
````

## Try with Azure CLI

```cli
# Create the Policy Definition (Subscription scope)
az policy definition create --name "deploynotexists-mysql-diaglogs-eventhub" --display-name "Deploy MySQL Diagnostic Settings with Event Hub" --description "This policy will deploy MySQL diagnostic settings where the resource does not have them enabled and pointing to the specified event hub" --rules "https://raw.githubusercontent.com/carluu/AzurePolicy/master/MySQL/deploynotexists-mysql-diaglogs-eventhub/azurepolicy.rules.json" --params "https://raw.githubusercontent.com/carluu/AzurePolicy/master/MySQL/deploynotexists-mysql-diaglogs-eventhub/azurepolicy.parameters.json" --mode All

# Create the Policy Assignment
az policy assignment create --name "deploynotexists-mysql-diaglogs-eventhub-assignment" --display-name "Deploy MySQL Diagnostic Settings with Event Hub Assignment" --scope <scope> --policy deploynotexists-mysql-diaglogs-eventhub --params @.\parametervalues.json --assign-identity --location <location>
```

## Deployment Notes
- Deployments above assume that there is a local file called parametervalues.json containing the values for the parameters
- If you want the managed identity to be granted the role to perform the action automatically, add:  
--> Powershell: Not available automatically. See here for manual steps: https://docs.microsoft.com/en-us/azure/governance/policy/how-to/remediate-resources#manually-configure-the-managed-identity  
--> CLI: --identity-scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --role Contributor