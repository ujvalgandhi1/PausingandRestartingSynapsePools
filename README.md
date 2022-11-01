# Github repo for code around pausing and restarting Synapse Pools

There is a need to pause and restart Synapse SQL Pools on a set schedule to save money. Having the Synapse Pool run 24x7 (There is no idle setting in Synapse SQL Pool unlike Spark Pools) is going to increase the total cost of ownership by a fair bit

Approach 1 - Using Azure Functions 
(https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview)

Azure Functions is a serverless solution that allows you incorporate PowerShell scripts to start/pause Synapse SQL Pools on a schedule
![FunctionApp1](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/FunctionApp-1.png)

Once the Function App is provisioned in your subscription/resource group, follow the screenshot below (4 steps) to provision two Timer Triggers. The timers are set at 0 0 8 * * * (Run the Function at 8 AM in your TimeZone)

![FunctionApp2](https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/FunctionApp-2.png)

Once you have the timer trigger set up, go under Code + Test and individually copy the code from the two word documents in the Assets folder. 
a. Start Synapse Pool using Azure Functions.docx for Starting Synapse Pools
b. Stop Synapse Pool using Azure Functions.docx for Stopping Synapse Pools
For both the codes, make the changes to reflect your subscription values like Subscription Name, Resource Group Name and Synapse Workspace Name



