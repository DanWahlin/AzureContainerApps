## Lab 6 - Continuous Integration and Observability with Azure Container Apps

In this lab you'll learn how to deploy Azure Container Apps using GitHub Actions and how to observe container apps.

### Exercise 1: Deploy a Dapr application with GitHub Actions for Azure Container Apps

In this exercise you'll:

- Configure a GitHub Actions workflow for deploying the end-to-end solution to Azure Container Apps.
- Modify the source code with a revision-scope change to trigger the Build and Deploy GitHub workflow.
- Learn how revisions are created for container apps in multi-revision mode.

1. Run the following command to list your default subscription (ensure you're using the correct subscription for this class). Notice the subscription `id` value.

    ```bash
    az account list --query "[?isDefault]"
    ```

    > Note: You can also get the `subscription id` using the following command:

    ```bash
    az account list --query "[?isDefault].id" -o tsv
    ```

1. Copy the subscription `id` value into your clipboard.

1. Follow the instructions at https://learn.microsoft.com/azure/container-apps/dapr-github-actions to setup GitHub to support container apps. Please consider the following items as you go through the tasks:

    1. When running the `az ad sp create-for-rbac` command to create a service principle, the service principle name value must be unique. Use a value such as `TestServicePrinciple<YOUR-FIRST-AND-LAST-NAME>` (replace `<YOUR-FIRST-AND-LAST-NAME>` with your first and last name of course).

    1. Once you fork and then clone the repository, open the `deploy/main.bicep` file in an editor and ensure that the `isPrivateRegistry` parameter at around line 20 is set to `false`. 
    
        > Note: There is a [PR](https://github.com/Azure-Samples/container-apps-store-api-microservice/pull/32) submitted to fix this issue but it may not be approved by the time you go through this exercise.

    1. When you go to `Build and Deploy` your workflow you'll see the following message in GitHub:

        ```
        Workflows aren’t being run on this forked repository
        Because this repository contained workflow files when it was forked, we have disabled them from running on this fork. Make sure you understand the configured workflows and their expected usage before enabling Actions on this repository.
        ```
        
        Select the `I understand my workflows, go ahead and enable them` button and then follow the steps in the tutorial to run the workflow.

    1. It'll take a few minutes for the workflow to deploy so wait until it completes before going to the `Verify the deployment` section of the tutorial.

### Exercise 2: Container App Observability

1. Run the following command to view TBD...