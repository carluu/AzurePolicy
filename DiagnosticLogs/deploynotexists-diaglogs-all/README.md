# Deploy diagnostic settings to all resource types specified

This policy is used to deploy diagnostic settings where specified.  
  
The policy takes in the following arguments for log destinations:  
- Event Hub ID: The event hub to send logs to
- Storage Account ID: The storage account to send logs to
- Workspace ID: The log analytics workspace to send logs to  
  
Note that for the above, all are optional (however if you deploy with none set, deployment will fail). When deploying, the state will try to match exactly the state specified, so if you previously had, for example, Storage and Event Hub set and deploy with Storage and Workspace, Event Hub will be unset and the final state will be just Storage and Workspace  
  
The policy also takes:  
- Resource Types: This is an array of the resource types you want to deploy diagnostic settings to
- Template source: This is the base URL (trailing slash not included) pointing to the deployment templates (see below for more information)  
  
## Deployment Templates  
Since ARM does not allow dyanmic specification of Type in a deployment, to achieve the goal of this policy, linked templates were required. For every resource assessed, if the policy needs to trigger a deployment, it will invoked a deployment using a linked template based on the resource type of the resource being assessed. For example, if the resource type is a MySQL database, the policy will translate the resource type from 'Microsoft.DBforMySQL/servers' to 'Microsoft.DBForMySQL-servers' (replacing '/' with '-')  look for a linked template at: <templateSource>/<convertedResourceType>.json. This means that for every resource type, there needs to be an associated deployment template. See the arm-templates folder under DiagnosticLogs for examples.

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
az policy definition create --name "deploynotexists-diaglogs-eventhub" --display-name "Deploy Resource Diagnostic Settings with Event Hub" --description "This policy will deploy diagnostic settings where the resource does not have them enabled and pointing to the specified event hub" --rules "https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticSettings/deploynotexists-diaglogs-eventhub/azurepolicy.rules.json" --params "https://raw.githubusercontent.com/carluu/AzurePolicy/master/DiagnosticSettings/deploynotexists-diaglogs-eventhub/azurepolicy.parameters.json" --mode All

# Create the Policy Assignment
az policy assignment create --name "deploynotexists-diaglogs-eventhub-assignment" --display-name "Deploy Resource Diagnostic Settings with Event Hub Assignment" --scope <scope> --policy deploynotexists-diaglogs-eventhub --params @.\parametervalues.json --assign-identity --location <location>
```

## Deployment Notes
- Deployments above assume that there is a local file called parametervalues.json containing the values for the parameters
- If you want the managed identity to be granted the role to perform the action automatically, add:  
--> Powershell: Not available automatically. See here for manual steps: https://docs.microsoft.com/en-us/azure/governance/policy/how-to/remediate-resources#manually-configure-the-managed-identity  
--> CLI: --identity-scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --role Contributor