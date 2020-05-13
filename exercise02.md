### Build and run the Claims API microservice locally : 45 min
In this section, we will work on the following tasks
- Configure the Azure SQL Server connection string in the Claims API microservice (source code)
- Build the Claims API microservice using the .NET Core 3.0 SDK
- Run the .NET Core *Entity Framework* migrations to create the relational database tables in Azure SQL Server provisioned in [Section A](/exercise01.md).  These tables will be used to persist Claims records.
- Run the Claims API microservice locally using the .NET Core 3.0 SDK
- Build the microservice Docker container and run the container

1.  Fork this [GitHub repository](https://github.com/tas-mac/aks-aspnet-sqldb-rest) to **your** GitHub account.

    In the browser window, click on **Fork** in the upper right hand corner to get a separate copy of this project added to your GitHub account.  You must be signed in to your GitHub account in order to fork this repository.

    ![alt tag](./images/B-01.PNG)

2.  Verify .netCore 3.0 on your machine as well the database migration tools. Check .NET Core version (Should print 3.0.100)
    ```bash
    $ dotnet --version
    
    # Install EF Core 3.0, this tool will be used for migration of database tables 
    $ dotnet tool install --global dotnet-ef --version 3.0
      ```

3.  Update the Azure SQL Server database connection string value in the **appsettings.json** file.

    The attribute **SqlServerDb** holds the database connection string and should point to the Azure SQL Server database instance which we provisioned in [Section A](#/exercise01).  You should have saved the SQL Server connection string value in a file.
    
    Edit the `appsettings.json` file 

    Variable Token | Description
    -------------- | -----------
    SQL_SRV_PREFIX | Name of the Azure SQL Server instance. Eg., <Your_Initial>sqldb
    SQL_USER_ID | User ID for the SQL Server instance
    SQL_USER_PWD | User password for the SQL Server instance 

    Do not include the curly braces and the hash symbols (#{ xxx }#) when specifying the values.  See the screenshot below.

    ![alt tag](./images/C-01.PNG)

2.  Build the Claims API microservice using the .NET Core SDK.

    ```bash
    # Build the Claims API microservice
    $ dotnet build
    ```

3.  Create and run the database migration scripts. 
  
    This step will create the database tables for persisting *Claims* records in the Azure SQL Server database.  Refer to the command snippet below.
    ```bash
    # Run the .NET Core CLI command to create the database migration scripts
    $ dotnet-ef migrations add InitialCreate
    #
    # Run the ef (Entity Framework) migrations
    $ dotnet-ef database update
    #
    ```

    Login to the Azure Portal and check if the database tables have been created. See screenshot below.

    ![alt tag](./images/C-02.PNG)

4.  Run the Claims API locally using the .NET Core SDK.

    Run the Claims API microservice from a powershell window.  Refer to the commands below.
    
    ```bash
    # Make sure you are in the Claims API source code directory
    /home/labuser/git-repos/aks-aspnet-sqldb-rest
    # Run the microservice
    $ dotnet run
    # When you are done testing:
    #   Press 'Control + C' to exit the program and return to the terminal prompt ($)
    #
    ```

    Use Postman or the browser to invoke the Claims API end-point.  Refer to the command snippet below.
    ```bash
    #   
    $ http://localhost:5000/api/v1/claims
    #
    ```

    The API end-point should return a 200 OK HTTP status code and also return one claim record in the HTTP response body. 
    
5.  Build and run the Claims API with Docker for Linux containers.
    ```bash
    # Run the docker build.
    # The build will take a few minutes to download both the .NET core build and run-time 
    # containers!
    # NOTE:
    # The 'docker build' command does the following (Review the 'dockerfile'):
    # 1. Build the dotnet application
    # 2. Layer the application binaries on top of a base container image
    # 3. Create a new application container image
    # 4. Save the built container image on the host (local machine)
    #
    # DO NOT forget the dot '.' at the end of the 'docker build' command !!!!
    # The '.' is used to set the context directory (path to the dockerfile) for the docker build.
    #
    $ docker build -t claims-api .
    #
    # List the docker images on this VM.  You should see two container images -
    # - mcr.microsoft.com/dotnet/core/sdk
    # - mcr.microsoft.com/dotnet/core/aspnet
    # Compare the sizes of the two dotnet container images and you will notice the size of the runtime image is pretty small ~ 207MB when compared to the 'build' container image ~ 689MB.
    $ docker images
    #
    # (Optional : Best Practice) Delete the intermediate .NET Core build container as it will consume unnecessary 
    # space on your build system (VM).
    $ docker rmi $(docker images -f dangling=true -q)
    #
    # Run the application container
    $ docker run -it --rm -p 80:80 --name test-claims-api claims-api
    #
    # When you are done testing:
    #   Press 'Control + C' to exit the program and return to the terminal prompt ($)
    #
    ```
