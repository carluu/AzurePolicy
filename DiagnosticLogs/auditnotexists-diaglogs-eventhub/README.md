# Audit Resource Diagnostic Settings to specific Event Hub

This policy audits that diagnostic settings have been enabled on all resource types passed in and are sending to a specified event hub

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FMySQL%2Fauditnotexists-mysql-diaglogs-eventhub%2Fazurepolicy.json)
[![Deploy to Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FMySQL%2Fauditnotexists-mysql-diaglogs-eventhub%2Fazurepolicy.json)

## Try with Azure PowerShell

````powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'auditnotexists-diaglogs-eventhub' -DisplayName 'Audit Resource Diagnostic Settings to specific Event Hub' -description 'This policy audits that diagnostic settings have been enabled on all resource types passed in and are sending to a specified event hub' -Policy 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-eventhub/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-eventhub/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'auditnotexists-diaglogs-eventhub-assignment' -DisplayName 'Audit Resource Diagnostic Settings to specific Event Hub Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter .\parametervalues.json
````

## Try with Azure CLI

```cli
# Create the Policy Definition (Subscription scope)
az policy definition create --name "auditnotexists-diaglogs-eventhub" --display-name "Audit Resource Diagnostic Settings to specific Event Hub" --description "This policy audits that diagnostic settings have been enabled on all resource types passed in and are sending to a specified event hub" --rules "https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-eventhub/azurepolicy.rules.json" --params "https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-eventhub/azurepolicy.parameters.json" --mode All

# Create the Policy Assignment
az policy assignment create --name "auditnotexists-diaglogs-eventhub-assignment" --display-name "Audit Resource Diagnostic Settings to specific Event Hub Assignment" --scope <scope> --policy auditnotexists-diaglogs-eventhub --params @.\parametervalues.json
```

## Deployment Notes
Deployments above assume that there is a local file called parametervalues.json containing the values for the parameters