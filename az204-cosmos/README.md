# Training
Learn  Training  Browse  Work with Azure Cosmos DB 
Exercise: Create resources by using the Microsoft .NET SDK v3
Completed
100 XP
15 minutes
In this exercise, you create a console app to perform the following operations in Azure Cosmos DB:

Connect to an Azure Cosmos DB account
Create a database
Create a container
Prerequisites
An Azure account with an active subscription. If you don't already have one, you can sign up for a free trial at https://azure.com/free.

Visual Studio Code on one of the supported platforms.

.NET 8 is the target framework for the exercise.

The C# extension for Visual Studio Code.

The latest Azure CLI tools, installed locally.

Setting up
Perform the following actions to prepare Azure, and your local environment, for the exercise.

Connecting to Azure
Start Visual Studio Code and open a terminal window by selecting Terminal from the top application bar, then choosing New Terminal.

Sign in to Azure by using the following command. A browser window should open letting you choose which account to sign in with.


Copy
az login
Create resources in Azure
Create a resource group for the resources needed for this exercise. Replace <myLocation> with a region near you.


Copy
az group create --location <myLocation> --name az204-cosmos-rg
Create the Azure Cosmos DB account. Replace <myCosmosDBacct> with a unique name to identify your Azure Cosmos DB account. The name can only contain lowercase letters, numbers, and the hyphen (-) character. It must be between 3-31 characters in length. This command takes a few minutes to complete.


Copy
az cosmosdb create --name <myCosmosDBacct> --resource-group az204-cosmos-rg
Record the documentEndpoint shown in the JSON response for use later in the exercise.

Retrieve the primary key for the account by using the following command. Record the primaryMasterKey from the command results for use in the code.


Copy
# Retrieve the primary key
az cosmosdb keys list --name <myCosmosDBacct> --resource-group az204-cosmos-rg
Set up the console application
Now that the needed resources are deployed to Azure the next step is to set up the console application using the same terminal window in Visual Studio Code.

Create a folder for the project and change in to the folder.


Copy
md az204-cosmos
cd az204-cosmos
Create the .NET console app.


Copy
dotnet new console
Open the current folder in Visual Studio Code using the following command. The -r option opens the folder without launching a new Visual Studio Code window.


Copy
code . -r
Select the Program.cs file in the Explorer pane to open the file in the editor.

Build the console app
It's time to start adding the packages and code to the project.

Add packages and using statements
Open the terminal in Visual Studio Code and use the following command to add the Microsoft.Azure.Cosmos package to the project.


Copy
dotnet add package Microsoft.Azure.Cosmos
Delete any existing code in the Program.cs file and add the using Microsoft.Azure.Cosmos statement.

C#

Copy
using Microsoft.Azure.Cosmos;
Add code to connect to an Azure Cosmos DB account
Add the following code snippet after the using statement. The code snippet adds constants and variables into the class and adds some error checking. Be sure to replace the placeholder values for EndpointUri and PrimaryKey following the directions in the code comments.

C#

Copy
public class Program
{
    // Replace <documentEndpoint> with the information created earlier
    private static readonly string EndpointUri = "<documentEndpoint>";

    // Set variable to the Primary Key from earlier.
    private static readonly string PrimaryKey = "<your primary key>";

    // The Cosmos client instance
    private CosmosClient cosmosClient;

    // The database we will create
    private Database database;

    // The container we will create.
    private Container container;

    // The names of the database and container we will create
    private string databaseId = "az204Database";
    private string containerId = "az204Container";

    public static async Task Main(string[] args)
    {
        try
        {
            Console.WriteLine("Beginning operations...\n");
            Program p = new Program();
            await p.CosmosAsync();

        }
        catch (CosmosException de)
        {
            Exception baseException = de.GetBaseException();
            Console.WriteLine("{0} error occurred: {1}", de.StatusCode, de);
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: {0}", e);
        }
        finally
        {
            Console.WriteLine("End of program, press any key to exit.");
            Console.ReadKey();
        }
    }
    //The sample code below gets added below this line
}
Below the Main method, add a new asynchronous task called CosmosAsync, which instantiates our new CosmosClient and adds code to call the methods you add later to create a database and a container.

C#

Copy
public async Task CosmosAsync()
{
    // Create a new instance of the Cosmos Client
    this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

    // Runs the CreateDatabaseAsync method
    await this.CreateDatabaseAsync();

    // Run the CreateContainerAsync method
    await this.CreateContainerAsync();
}
Create a database
Copy and paste the CreateDatabaseAsync method after the CosmosAsync method. CreateDatabaseAsync creates a new database with ID az204Database if it doesn't already exist.

C#

Copy
private async Task CreateDatabaseAsync()
{
    // Create a new database using the cosmosClient
    this.database = await this.cosmosClient.CreateDatabaseIfNotExistsAsync(databaseId);
    Console.WriteLine("Created Database: {0}\n", this.database.Id);
}
Create a container
Copy and paste the CreateContainerAsync method below the CreateDatabaseAsync method.

C#

Copy
private async Task CreateContainerAsync()
{
    // Create a new container
    this.container = await this.database.CreateContainerIfNotExistsAsync(containerId, "/LastName");
    Console.WriteLine("Created Container: {0}\n", this.container.Id);
}
Run the application
Save your work and, in a terminal in Visual Studio Code, check for any errors by running the dotnet build command. Run the dotnet run command if the build is successful. The console displays the following messages.


Copy
Beginning operations...

Created Database: az204Database

Created Container: az204Container

End of program, press any key to exit.
Verify the results by opening the Azure portal, navigating to your Azure Cosmos DB resource, and use the Data Explorer to view the database and container.

Clean up Azure resources
You can now safely delete the az204-cosmos-rg resource group from your account by running the following command.


Copy
az group delete --name az204-cosmos-rg --no-wait
Next unit: Create stored procedures
 

Having an issue? We can help!

For issues related to this module, explore existing questions using the #azure training tag or Ask a question on Microsoft Q&A.
For issues related to Certifications and Exams, post on Certifications Support Forums or visit our Credentials Help.
Feedback
Was this page helpful?

Previous Versions
Blog
Contribute
Privacy
Terms of Use
Trademarks
© Microsoft 2025
