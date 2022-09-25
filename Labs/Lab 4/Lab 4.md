## Lab 4 - Microservices and Azure Container Apps

In this lab you'll learn how Azure Container Apps can be used to deploy microservices.

### Exercise 1: Deploy your Code to Azure Container Apps

In this exercise you'll deploy a simple microservice API to Azure Container Apps. 
 
1. Perform the steps in the following tutorial to deploy a microservice API to Azure Container Apps. 

    https://docs.microsoft.com/en-us/azure/container-apps/quickstart-code-to-cloud?tabs=bash%2Ccsharp&pivots=acr-remote

    > Note: You can define the shell variables mentioned in the tutorial using one of the following techniques:

    ```bash
    # bash/sh
    <VARIABLE_NAME>=<VARIABLE_Value>
    ```

    ```powershell
    # PowerShell
    $<VARIABLE_NAME>=<VARIABLE_Value>
    ```

1. SKIP the `Clean up resources` step at the end of the tutorial since you'll need the `album-api` container app in the next exercise.

1. Keep your terminal window open and continue to the next exercise.

### Exercise 2: Communication between Microservices in Azure Container Apps

In this exercise you'll deploy a UI application that consumes the API container app created in the previous exercise. You'll understand how to locate the endpoint for the microservice API and pass that to the application so it can communicate.

1. Perform the steps from the following tutorial to learn how to communicate between microservices:

    https://docs.microsoft.com/en-us/azure/container-apps/communicate-between-microservices?source=recommendations&tabs=bash&pivots=acr-remote

1. SKIP the `Clean up resources` step at the end of the tutorial since you'll need the `album-ui` container app in the next exercise.

1. Keep your terminal window open and continue to the next exercise.

### Exercise 3: Using the Azure Portal to Scale a Microservice

1. Open the Azure Portal and navigate to your `album-api` container app.

1. Change the scale of the `album-api` container app to allow a minimum of 1 and a maximum of 5.

1. Add an HTTP scale trigger to the `album-api` container app. Use a value of 50 for the `Concurrent requests`.

### Exercise 4: Build a New Image and Modify Traffic to the Revisions

1. In an earlier exercise you cloned the `album-ui` repository. 

1. Open the `containerapps-albumui/src/views/index.pug` file in an editor.

1. Change the `span(class="album-title")=album.title` line to the following:

    ```pug
    span(class="album-title") New and Improved Title!
    ```

1. Build the image using ACR and give it a tag of `2.0`:

    ```bash
    az acr build --registry $ACR_NAME --image albumapp-ui:2.0 .
    ```

1. Change the `album-ui` container app to support `multiple revisions` using either the Azure Portal or the Azure CLI.

1. Create a new `album-ui` container app revision that uses the `albumapp-ui:2.0` container image using either the Azure Portal or the Azure CLI. Give the revision a suffix value of `revision-2`.

1. View the revisions for the container app using the Azure Portal or the Azure CLI.

1. Split the traffic 50/50 between the two revisions in the Azure Portal or by using the Azure CLI `az containerapp ingress traffic set` command.

1. Visit the URL for the container app and refresh until you see the modified title.

1. Clean up the resources by running the following command:

    ```bash
    az group delete --name $RESOURCE_GROUP
    ```



