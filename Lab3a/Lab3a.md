# Lab 3 - Process data using Databricks

## Introduction

In this lab we will transform some of our data using Databricks. This will be achieved running SparkSQL commands in an interactive notebook environment. 

## Provision the Databricks Workspace

To get started, first create a Databricks workspace in the East US region. Make use of the trial pricing tier. 

![Databricksworkspace.png](images/Databricksworkspace.png)

Note you are only billed for the time your cluster is running, and you can use the pricing calculator to get an idea of costs. Essentially though you’re paying for the underyling VMs (which are provisioned in an automatically created resource group) and DBUs which are processing units per hour. If you take the free trial, you will only pay for the VMs.

## Provision Key Vault
 
You will also need an Azure Key Vault to store secrets. Create a key vault as shown in the diagram below.

![KeyVault.png](images/KeyVault.png)

## Accessing ADLS or Blob from Databricks

If you have access to your AAD to create an application & service principle then use ADLS otherwise use the storage account created by the initial deployment script. If using [Azure Data Lake Storage with Databricks](https://docs.databricks.com/spark/latest/data-sources/azure/azure-datalake-gen2.html#azure-data-lake-storage-gen2) you will require a service principal with delegated permissions. Start by creating a [Gen2 storage account](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-quickstart-create-account) with [hierarchical namespace](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-namespace) enabled. Once created, go the resource and scroll down the Data Lake Storage section and create a new container/filesystem called "datalake".

Follow the steps in the [documentation](https://docs.databricks.com/data/data-sources/azure/azure-datalake-gen2.html#create-and-grant-permissions-to-service-principal) to create a service principal by registering an application and then assigning the “Storage Blob Data Contributor” role to the service principal (the registered application) at the level of the resource i.e. in the access control of the data lake storage account. Also follow the steps to create a [new application secret](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret) which will be stored later in Key Vault. If you're using Blob storage, you will use the access keys for the storage account.

## Import the Databricks notebooks
By now your Databricks workspace should be ready, find the resource and click Launch Workspace. A new tab will open and the Databricks workspace landing page will be displayed. You may be prompted for your Azure credentials again.

![Databricks.png](images/Databricks.png)

Next import the notebooks required for this lab. Click on the Home icon, and click on the drop menu next to your username to reveal the import option as shown below. 

![ImportNotebooksm.png](images/ImportNotebooksm.png)

Import from the following archive: 

https://github.com/hurtn/DataLakeInADay/blob/master/Lab3a/DataLakeInADay.dbc

If successful you will notice a new folder entitled DataLakeInADay appear, which contains a set of notebooks and some sub-folders.

## Store your secrets in Key Vault
Before proceeding any further, let's add your secret to the Key Vault provisioned earlier. Navigate to this resource, open the Secrets blade and click Generate/Import. Enter a name for the secret associated with the service principle created earlier or the storage account access key depending which storage option you're using. For convenience, use a secret name of "spsecret" which will be referenced later in your Databricks notebook later. 

## Secret Scope
Instead of using secrets in your code it is recommended to use Secrets scope backed by keyvault. To configure Databricks to access Key vault open the URL below in a new tab.
https://eastus.azuredatabricks.net#secrets/createScope

Follow the [documentation](https://docs.azuredatabricks.net/security/secrets/secret-scopes.html#create-an-azure-key-vault-backed-secret-scope) to complete this configuration. For convenience, use a scope name of "dliadsecrets". The DNS and Resource ID parameters can be found in the Properties blade of the Key Vault resource. 

## Databricks Clusters
In order to interact with the Databricks notebook we will need a running cluster. In the Databricks workspace, navigate to the Clusters page. Click create cluster, and create a cluster with all defaults except untick autoscale and specify 2 worker nodes.

![cluster.png](images/clustersm.png)

## Mount Azure Storage
While the cluster starts up (5-8 mins) open the first notebook within the DataLakeInADay which is 0.Mount filesystem. Attached the starting cluster using the drop down under the title of the notebook.

![startup.png](images/startupsm.png)

Whether you are using Blob or Data Lake storage review the appropriate section and read the associated documentation links for that section. Next ensure that the dbutils secret API request contains the correct parameters.

dbutils.secrets.get(scope = "<scope-name>", key = "<secret>"
 
Once the cluster has started the icon will change to solid green

![clusterstarted.png](images/clusterstarted.png)

To run the particular command, click the run icon or use ctrl-enter.

# Next

Now you can go on to [Lab 4](../Lab4/Lab4.md)
