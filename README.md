# Github repo for code around pausing and restarting Synapse Pools

There is a need to pause and restart Synapse SQL Pools on a set schedule to save money. Having the Synapse Pool run 24x7 (There is no idle setting in Synapse SQL Pool unlike Spark Pools) is going to increase the total cost of ownership by a fair bit

Approach 1 - Using Azure Functions 
(https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview)

Azure Functions is a serverless solution that allows you incorporate PowerShell scripts to start/pause Synapse SQL Pools on a schedule
[FunctionApp1](!https://github.com/ujvalgandhi1/PausingandRestartingSynapsePools/blob/main/Assets/FunctionApp-1.png)
