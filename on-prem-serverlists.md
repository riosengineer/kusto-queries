# Group on premise servers in Log Analytics functions to report and alert from

### Intro 
This is how you can group together on premise servers reporting into Log Analytics for query functions in kusto queries. This can be useful if you want to only report on 'domain controllers' for dashboarding and alerts. 

You should have already installed the OMS agent on your servers and confirmed heartbeats are registering in the workspace you configured

https://docs.microsoft.com/en-us/azure/azure-monitor/agents/agent-windows
#### Blob CSV List
Upload a CSV file to a blob storage account with two columns 'App' and 'ServerName'. Here you can list each server you wish to group together by function, i.e. databases, proxies, domain controllers, and so forth. 

#### SAS Token 

Create a SAS token which we will need to use in the kusto query 

https://docs.microsoft.com/en-us/azure/cognitive-services/translator/document-translation/create-sas-tokens

#### Kusto Query

Use the sas token URL in the query example like below (example given in query). You can tweak the query to suit your reporting or alert needs by amending the `| where App == ' '` 

    externaldata(App:string, ServerName:string) [@"https://storageaccountname.blob.core.windows.net/containername/filename.csv?SASTOKEN"]
        | join kind= inner (
           Heartbeat
           | where TimeGenerated >= ago(30d)  
        ) on $left.ServerName == $right.Computer
        | where App == 'databases'
        | distinct Computer

##
You can also save the query as a function and name it appropriately, and then use the function to query the results, e.g. 'databases' function would now return all on premise databases and we can create a dashboard for these
