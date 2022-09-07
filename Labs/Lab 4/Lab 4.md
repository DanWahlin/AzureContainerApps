## Lab 5 - Microservices and Azure Container Apps

In this lab you'll learn how Azure Container Apps can be used to deploy microservices.

### Exercise 1: Deploying the Reddog application to Azure Container Apps
 
1. Perform the steps below to deploy the Reddog application and microservices into Azure Container Apps:

    https://github.com/Azure/reddog-containerapps#deployment

    > Note: This deployment can take awhile to finish so please be patient and leave your console running until it completes.

### Exercise 2: Exploring and Scaling Reddog Microservices

1. Run the following command to get the URL for the Reddog application:

    ```bash
    az deployment group show -n reddog -g $RG -o json --query properties.outputs.urls.value
    ```

1. View the Reddog URL in your browser to ensure the app is up and running. It may take a few moments for it to load the first time you visit the site.

    > Note: If you have any issues, go to the Azure Portal, go locate the `reddog` resource group and select the `ui` container app. Go into revision management and ensure it was `provisioned` successfully. You can click on the revision name to view `System logs` if there was an issue with the deployment.

1. Use the Azure CLI to perform the following tasks:
    - Get the name of the Reddog environment.
    - List all of the container apps in the environment.



