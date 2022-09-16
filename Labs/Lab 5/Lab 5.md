## Lab 5 - Dapr and Microservices with Azure Container Apps

In this lab you'll learn how Dapr and microservices can be used with Azure Container Apps.

### Pre-Exercise: Deploying the Reddog application to Azure Container Apps

1. Clone the `Reddog` application source code to your machine:

    ```bash
    git clone https://github.com/Azure/reddog-code
    ```

1. Clone the `Red Dog Demo: Azure Container Apps Deployment` project:

    ```bash
    https://github.com/Azure/reddog-containerapps
    ```
 
1. Perform the steps below to deploy the Reddog application and microservices into Azure Container Apps:

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

    > These steps can be found at https://github.com/Azure/reddog-containerapps#deployment

    > Note: This deployment can take awhile to finish so please be patient and leave your console running until it completes.

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

1. Open the `reddog-code` folder (you cloned this in Exercise 1) in an editor.

### Exercise 2: Deploy a Dapr Application and Component to Azure Container Apps using the Azure CLI

1. Perform the steps at https://docs.microsoft.com/azure/container-apps/microservices-dapr to:
    - Create a Container Apps environment for your container apps
    - Create an Azure Blob Storage state store for the container app
    - Deploy two apps that produce and consume messages and persist them in the state store
    - Verify the interaction between the two microservices.





