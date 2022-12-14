## Lab 5 - Dapr and Microservices with Azure Container Apps

In this lab you'll learn how Dapr and microservices can be used with Azure Container Apps.

### Pre-Exercise: Deploying the Reddog application to Azure Container Apps

1. If you have an existing Container Apps environment in your subscription delete it before performing the following steps.

1. Clone the `Red Dog Demo: Azure Container Apps Deployment` project:

    ```bash
    https://github.com/Azure/reddog-containerapps
    ```

1. `cd` into the `reddog-containerapps` folder.

1. Run the following command to list your default subscription (ensure you're using the correct subscription for this class). Notice the subscription `id` value.

    ```bash
    az account list --query "[?isDefault]"
    ```

    > Note: You can also get the `subscription id` using the following command:

    ```bash
    az account list --query "[?isDefault].id" -o tsv
    ```

1. Copy the subscription `id` value into your clipboard.

1. Perform the steps below to deploy the Reddog application and microservices into Azure Container Apps. Replace `SUB_ID` with the subscription id you copied into your clipboard in the previous step.

    ```bash
    # *nix only
    export RG="reddog"
    export LOCATION="eastus2"
    export SUB_ID="<YourSubscriptionID>"

    # Follow Azure CLI prompts to authenticate to the subscription of your choice
    az login
    az account set --subscription $SUB_ID

    # Create resource group
    az group create -n $RG -l $LOCATION

    # Deploy infrastructure and reddog apps
    az deployment group create -n reddog -g $RG -f ./deploy/bicep/main.bicep

    # Display outputs from bicep deployment
    az deployment group show -n reddog -g $RG -o json --query properties.outputs.urls.value
    ```

    > Note: These steps can be found at https://github.com/Azure/reddog-containerapps#deployment    
    This deployment can take awhile to finish so please be patient and leave your console running until it completes.

### Exercise 1: Exploring and Reddog Microservices

1. Run the following command to get the URL for the Reddog application:

    ```bash
    az deployment group show -n reddog -g $RG -o json --query properties.outputs.urls.value
    ```

1. View the Reddog URL in your browser to ensure the app is up and running. It may take a few moments for it to load the first time you visit the site.

    > Note: If you have any issues, go to the Azure Portal, go locate the `reddog` resource group and select the `ui` container app. Go into revision management and ensure it was `provisioned` successfully. You can click on the revision name to view `System logs` if there was an issue with the deployment.

1. Use the Azure CLI to perform the following tasks:
    - Get the name of the Reddog environment.
    - List all of the container apps in the environment.

1. Take a moment to look through the following table that summarizes the services in the project:

    | Service          | Ingress |  Dapr Component(s) | KEDA Scale Rule(s) |
    |------------------|---------|--------------------|--------------------|
    | Traefik | External | Dapr not enabled | HTTP |
    | UI | Internal | Dapr not enabled | HTTP |
    | Virtual Customer | None | Service to Service Invocation | N/A |
    | Order Service | Internal | PubSub: Azure Service Bus | HTTP |
    | Accounting Service | Internal | PubSub: Azure Service Bus | Azure Service Bus Subscription Length, HTTP |
    | Receipt Service | Internal | PubSub: Azure Service Bus, Binding: Azure Blob | Azure Service Bus Subscription Length |
    | Loyalty Service | Internal | PubSub: Azure Service Bus, State: Azure Cosmos DB | Azure Service Bus Subscription Length |
    | Makeline Service | Internal | PubSub: Azure Service Bus, State: Azure Redis | Azure Service Bus Subscription Length, HTTP |
    | Virtual Worker | None | Service to Service Invocation, Binding: Cron | N/A |

1. Open https://github.com/Azure/reddog-code/blob/master/RedDog.OrderService/Controllers/OrderController.cs#L24 and notice that a `DaprClient` object is injected into the constructor:

    ```csharp
    public OrderController(ILogger<OrderController> logger, DaprClient daprClient)
    {
        _logger = logger;
        _daprClient = daprClient;
    }
    ```

1. Locate the `NewOrder()` method and notice that it uses the `DaprClient` object to publish an event:

    ```csharp
    [HttpPost]
    public async Task<IActionResult> NewOrder(CustomerOrder order)
    {
        _logger.LogInformation("Customer Order received: {@CustomerOrder}", order);

        var orderSummary = await CreateOrderSummaryAsync(order);
        _logger.LogInformation("Created Order Summary: {@OrderSummary}", orderSummary);

        try
        {
            // Dapr used to publish an event
            await _daprClient.PublishEventAsync<OrderSummary>(PubSubName, OrderTopic, orderSummary);
            _logger.LogInformation("Published Order Summary: {@OrderSummary}", orderSummary);
        }
        catch(Exception e)
        {
            _logger.LogError("Error publishing Order Summary: {@OrderSummary}, Message: {Message}", orderSummary, e.InnerException?.Message ?? e.Message);
            return Problem(e.Message, null, (int)HttpStatusCode.InternalServerError);
        }

        return Ok();
    }
    ```

    > Note: You can find the Dapr Component definition for the pub/sub service (Azure ServiceBus) at https://github.com/Azure/reddog-code/blob/master/manifests/corporate/components/reddog.pubsub.yaml. It's important to note that Azure Container Apps currently uses a different format than the standard Dapr component spec.

1. Open https://github.com/Azure/reddog-code/blob/master/RedDog.LoyaltyService/Controllers/LoyaltyController.cs#L44 and note that a `DaprClient` object is used to get a state entry (from Cosmos DB in this case).

1. After the state entry is retrieved note that it is saved back to the store:

    ```csharp
    isSuccess = await stateEntry.TrySaveAsync(_stateOptions);
    ```

    > Note: You can find the Dapr Component definition for the state store at https://github.com/Azure/reddog-code/blob/master/manifests/corporate/components/reddog.state.loyalty.yaml.

1. Open https://github.com/Azure/reddog-code/blob/master/RedDog.ReceiptGenerationService/Controllers/ReceiptGenerationController.cs#L36 and note that the `DaprClient` is used to save data to a blob storage binding:

    ```csharp
    await daprClient.InvokeBindingAsync<OrderSummary>(ReceiptBindingName, "create", orderSummary, metadata);
    ```

    > Note: You can find the Dapr Component definition for the state store at https://github.com/Azure/reddog-code/blob/23586096d81077d1223b70d47e9a1cd395fab50c/manifests/branch/base/components/reddog.binding.receipt.yaml.

1. Open https://github.com/Azure/reddog-code/blob/master/RedDog.VirtualCustomers/VirtualCustomers.cs#L273 and notice that it uses `DaprClient` to call an order service.

### Bonus Exercise (if time permits): Deploy a Dapr Application and Component to Azure Container Apps using the Azure CLI

1. Perform the steps at https://docs.microsoft.com/azure/container-apps/microservices-dapr to:
    - Create a Container Apps environment for your container apps
    - Create an Azure Blob Storage state store for the container app
    - Deploy two apps that produce and consume messages and persist them in the state store
    - Verify the interaction between the two microservices.





