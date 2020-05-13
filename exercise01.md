## A. Deploy an Azure SQL Server and Database
**Approx. time to complete this section: 20 minutes**

In this section, we will create an Azure SQL Server instance and create a database (`ClaimsDB`).  This database will be used by the Claims API microservice to persist *Claims* records.

1.  Login to the Azure Cloud Shell.

    Login to the [Azure Portal](https://portal.azure.com) using your credentials and use a [Azure Cloud Shell](https://shell.azure.com) session to perform the next steps.  Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.  The first time you access the Cloud Shell, you will be prompted to create a resource group, storage account and file share.  You can use the defaults or click on *Advanced Settings* to customize the defaults.  Accessing the Cloud Shell is described in [Overview of Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview). 

2.  Take note of your Resource Group.

    Use Azure CLI to view your **Resource Group**.  You can also use Azure Portal to create this resource group.  
    ```bash
    $ az group list
    ```
    >**NOTE:** Keep in mind, if you specify a different name for the resource group (other than **myResourceGroup**), you will need to substitute the same value in multiple CLI commands in the remainder of this project!  If you are new to Azure Cloud, it's best to use the suggested name.

3.  Create an Azure SQL Server (managed instance) and database.

    In the Azure Portal, click on **+ Create a resource**, **Databases** and then click on **SQL Database** as shown in the screenshot below.

    ![alt tag](./images/A-01.PNG)
    
    In the **Create SQL Database** window **Basics** tab, select the resource group which you created in the previous step and provide a name for the database (`ClaimsDB`).  Then click on **Create new** besides field **Server**.

    ![alt tag](./images/A-02.PNG)

    Fill in the details in the **New Server** web page.  The **Server name** value should be unique as the SQL database server FQDN will be constructed with this name eg., <SQL_server_name>.database.windows.net. Use a pattern such as **<Your_Initial>sqldb** for the server name (Replace *Your_Initial* with your initials).

    Specify a **Server admin login** name.  Specify a simple **Password** containing numbers, lower and uppercase letters.  Avoid using special characters (eg., * ! # $ ...) in the password!

    For the **Location** field, use the same location which you specified for the resource group.

    See screenshot below.

    ![alt tag](./images/A-07.PNG)

    Click on **OK**.

    Click **Next**.

    Click on **Next : Networking >** at the bottom of the web page.  In the **Networking** tab, select *Public endpoint* for **Connectivity method**.  Also, enable the button besides **Allow Azure services and resources to access this server**.  Next, click on **Review + create**.  See screenshot below.

    ![alt tag](./images/A-12.jpg)

    Review and make sure all options you have selected are correct.

    ![alt tag](./images/A-11.PNG)

    Click **Review + Create**.  It will take approx. 10 minutes for the SQL Server instance and database to get created.

4.  Configure a firewall for Azure SQL Server.

    Once the SQL Server provisioning process is complete, click on the database `ClaimsDB`. In the **Overview** tab, click on **Set server firewall** as shown in the screenshot below.

    ![alt tag](./images/A-04.PNG)

    In the **Firewall settings** tab, configure a *rule* to allow inbound connections from all public IP addresses as shown in the screenshots below.  Alternatively, if you know the Public IP address of your workstation/pc, you can create a rule to only allow inbound connections from your local pc.  Leave the setting for **Allow access to Azure services** ON.  See screenshot below.

    ![alt tag](./images/A-05.PNG)

    Click on **Save**.

    >**NOTE**: Remember to delete the firewall rule setting once you have finished working on all labs in this project.

5.  Copy the Azure SQL Server database (ClaimsDB) connection string.

    In the **ClaimsDB** tab, click on **Connection strings** in the left navigational panel (blade).  See screenshot below.

    ![alt tag](./images/A-06.PNG)

    Copy the SQL Server database connection string under the **ADO.NET** tab.  In the connection string, remove the listener port address including the 'comma' (**,1433**) before saving this string in a file.  If the **comma** and the port number are present in the connection string then application deployments will **fail**.  We will need the SQL Server db connection string in the next sections to configure the SQL Server database for the Claims API microservice.

6.  Connect via Visual Studio Database plugin or via the SQL a mangement Studio to validate that you can login.

*** Done ***
