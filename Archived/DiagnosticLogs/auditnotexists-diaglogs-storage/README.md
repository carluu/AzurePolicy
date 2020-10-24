# Audit Resource Diagnostic Settings to specific Storage Account

This policy audits that diagnostic settings have been enabled on all resource types passed in and are sending to a specified storage account

## Try with Azure portal

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FDiagnosticLogs%2Fauditnotexists-diaglogs-storage%2Fazurepolicy.json)
[![Deploy to Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcarluu%2FAzurePolicy%2Fmaster%2FDiagnosticLogs%2Fauditnotexists-diaglogs-storage%2Fazurepolicy.json)

## Try with Azure PowerShell

````powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'auditnotexists-diaglogs-storage' -DisplayName 'Audit Resource Diagnostic Settings with Storage' -description 'This policy audits that diagnostic settings have been enabled on on all resource types passed in and are being directed to a specific storage account' -Policy 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-storage/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-storage/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'auditnotexists-diaglogs-storage-assignment' -DisplayName 'Audit Resource Diagnostic Settings with Storage Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter .\parametervalues.json
````

## Try with Azure CLI

```cli
# Create the Policy Definition (Subscription scope)
az policy definition create --name "auditnotexists-diaglogs-storage" --display-name "Audit Resource Diagnostic Settings with Storage" --description "This policy audits that diagnostic settings have been enabled on on all resource types passed in and are being directed to a specific storage account" --rules "https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-storage/azurepolicy.rules.json" --params "https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticLogs/auditnotexists-diaglogs-storage/azurepolicy.parameters.json" --mode All

# Create the Policy Assignment
az policy assignment create --name "auditnotexists-diaglogs-storage-assignment" --display-name "Audit Resource Diagnostic Settings with Storage Assignment" --scope <scope> --policy auditnotexists-diaglogs-storage --params @.\parametervalues.json
```

## Deployment Notes
Deployments above assume that there is a local file called parametervalues.json containing the values for the parameters