# Lab 3 - Process data using Databricks

## Introduction

In this lab we will transform some of our data using Databricks. This will be achieved running SparkSQL commands in an interactive notebook environment. 

To get started, first create a Databricks workspace in the East US region. Make use of the trial pricing tier. 

![Databricksworkspace.png](images/Databricksworkspace.png)

Note you are only billed for the time your cluster is running, and you can use the pricing calculator to get an idea of costs. Essentially though you’re paying for the underyling VMs (which are provisioned in an automatically created resource group) and DBUs which are processing units per hour. If you take the free trial, you will only pay for the VMs.
 
You will also need an Azure Key Vault to store secrets. Create a key vault as shown in the diagram below.

![KeyVault.png](images/KeyVault.png)

https://westus.azuredatabricks.net#secrets/createScope

By now your Databricks workspace should be ready, find the resource and click Launch Workspace. A new tab will open and the Databricks workspace landing page will be displayed. You may be prompted for your Azure credentials again.

![Databricks.png](images/Databricks.png)

Next import the notebooks required for this lab. Click on the Home icon, and click on the drop menu next to your username to reveal the import option. Select import from URL and copy-paste the following link

https://github.com/hurtn/DataLakeInADay/blob/master/Lab3a/DataLakeInADay.dbc

![ImportNotebooksm.png](images/ImportNotebooksm.png)




# Next

Now you can go on to [Lab 4](../Lab4/Lab4.md)
