## Lab 5 - Microservices and Azure Container Apps

In this lab you'll learn how Azure Container Apps can be used to deploy microservices.

### Exercise 1: Deploy your code to Azure Container Apps

In this exercise you'll deploy a simple microservice API to Azure Container Apps. 
 
1. Perform the steps in the following tutorial to deploy a microservice API to Azure Container Apps. 

    https://docs.microsoft.com/en-us/azure/container-apps/quickstart-code-to-cloud?tabs=bash%2Ccsharp&pivots=acr-remote

    > Note: SKIP the `Clean up resources` step at the end of the tutorial since you'll need the `album-api` container app in the next exercise.

1. Keep your terminal window open and continue to the next exercise.

### Exercise 2: Communication between microservices in Azure Container Apps

In this exercise you'll deploy a UI application that consumes the API container app created in the previous exercise. You'll understand how to locate the endpoint for the microsoft API and pass that to the application so that the two can communicate.

1. Perform the steps from the following tutorial to learn how to communicate between microservices:

    https://docs.microsoft.com/en-us/azure/container-apps/communicate-between-microservices?source=recommendations&tabs=bash&pivots=acr-remote

    > Note: SKIP the `Clean up resources` step at the end of the tutorial since you'll need the `album-api` container app in the next exercise.

1. Keep your terminal window open and continue to the next exercise.

### Exercise 3: Using the Azure Portal to Scale a Microservice

1. Open the Azure Portal and navigate to your `album-api` container app.

1. Change the scale of the `album-api` container app to allow a minimum of 1 and a maximum of 5.

1. Add an HTTP scale trigger to the `album-api` container app. Use a value of 50 for the `Concurrent requests`.

1. Clean up the resources by running the following command. If you accidentally closed your terminal window and lost the environment variables defined in Exercise 1, you can replace `$RESOURCE_GROUP` with `album-containerapps`.

    ```bash
    az group delete --name $RESOURCE_GROUP
    ```

