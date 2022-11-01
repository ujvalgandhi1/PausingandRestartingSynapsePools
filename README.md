# Github repo for code around pausing and restarting Synapse Pools

There is a need to pause and restart Synapse SQL Pools on a set schedule to save money. Having the Synapse Pool run 24x7 (There is no idle setting in Synapse SQL Pool unlike Spark Pools) is going to increase the total cost of ownership by a fair bit

**Approach 1 - Using Azure Functions**
(https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview)

Azure Functions is a serverless solution that allows you incorporate PowerShell scripts to start/pause Synapse SQL Pools on a schedule
![FunctionApp1](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/FunctionApp-1.png)

Once the Function App is provisioned in your subscription/resource group, follow the screenshot below (4 steps) to provision two Timer Triggers. The timers are set at 0 0 8 * * * (Run the Function at 8 AM in your TimeZone)

![FunctionApp2](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/FunctionApp-2.png)

Once you have the timer trigger set up, go under Code + Test and individually copy the code from the two word documents in the Assets folder. 
a. Start Synapse Pool using Azure Functions.docx for Starting Synapse Pools
b. Stop Synapse Pool using Azure Functions.docx for Stopping Synapse Pools
For both the codes, make the changes to reflect your subscription values like Subscription Name, Resource Group Name and Synapse Workspace Name



**Approach 2 - Using Azure Automation Account** 
(https://github.com/Azure/Azure-Functions/issues/193)

![Automation Runbook1](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/AutomationRunbook-1.png)

![Automation Runbook2](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/AutomationRunbook-2.png)

![Automation Runbook3](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/AutomationRunbook-3.png)

![Automation Runbook4](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/AutomationRunbook-4.png)

Follow the word documents AutomationRunbook-PauseSynapsePool.docx under the Assets folder and place it in the EDIT screen. Then follow instructions on Screenshot 4 to test the automation and finally publish it


**Approach 3 - Synapse Pipelines**
The Pipeline approach is the most expensive because it invovkes Synapse Pipelines to call in the REST APIs being used for Synapse management. However, it is the easisest to put together

![Pipeline-1](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/Pipeline-1.png)

![Pipeline-2](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/Pipeline-2.png)
1. Get Web Activity and rename it as "Get List"
2. Click the "Add Dynamic Content" under URL and paste this in the URL 
@concat('https://management.azure.com/subscriptions/',pipeline().parameters.SubscriptionID,'/resourceGroups/',pipeline().parameters.ResourceGroup,'/providers/Microsoft.Synapse/workspaces/',pipeline().parameters.WorkspaceName,'/sqlPools?api-version=2019-06-01-preview')
3. Use GET as Method
4. Create an User managed identity and for authentication and click the User Identity under the credentials

![Pipeline-3](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/Pipeline-3.png)
Get a For Each loop from the within the activities and then join the first "Get List" activity to this For Each activity

Make sure under Settings for For Each, you have the correct Paramaterized Item value as displayed

![Pipeline-4](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/Pipeline-4.png)

Under the For Each loop, add two Web activities and join them together

![Pipeline-5](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/Pipeline-5.png)
Follow the screen prompts to enter data for checking the state of the pool

For the Dynamic content for the URL, use this 
@concat('https://management.azure.com/subscriptions/',pipeline().parameters.SubscriptionID,'/resourceGroups/',pipeline().parameters.ResourceGroup,'/providers/Microsoft.Synapse/workspaces/',pipeline().parameters.WorkspaceName,'/sqlPools/',item().name,'?api-version=2019-06-01-preview')

