## Lab 2

In this lab you'll learn how to work with Azure Container Apps revisions.

> Tip: Use `az containerapp [command] --help`, visit https://docs.microsoft.com/cli/azure/containerapp, or view the `Solution.md` file to get help with Azure CLI commands.

### Exercise 1: Create an Azure Container App
 
1. Use the Azure CLI or Azure Portal to create a new Azure Container App named `customers-web`. HTTP ingress should be enabled and the container app should use the following properties:

    > Note: You'll need to create an Azure Container Apps `environment` if you don't already have one from the previous lab. See Lab 1 if you need more details.

    - environment: [your-container-apps-environment]
    - image: `nginx:1.17.8-alpine`
    - target-port: `80`
    - ingress: `external`
    - container-name: `customers-app`
    - min-replicas: 1 
    - max-replicas 5

    <br />

    #### Azure Portal Tip

    > If you're using the `Azure Portal`, ensure that you enable HTTP Ingress and select the `Accepting traffic from anywhere` option. Ensure that you set the `Target port` to `80`.

1. Once the app is created, view the `nginx` homepage in the browser to ensure the container app is running correctly.

1. Use the `Azure Portal` to list all revisions for the `customers-web` container app.

1. Use the `Azure CLI` to list all revisions for the `customers-web`. 

1. Copy the name of the current revision to your clipboard.

1. Using the Azure CLI, show the current revision (you copied the name into your clipboard in the previous step) as `yaml` and view the current `image` value.

1. Use the Azure CLI to `update` the revision `image` to use the following:

    - image: `nginx:1.23.1-alpine`
    - revision-suffix: `a-test`

1. Show the new revision in the console and ensure that the `image` value has been updated. Locate the `fqdn` value for the revision and view the URL in the browser to ensure the app is working properly.

1. List the `customers-web` container app revisions using the Azure CLI. Two revisions should now be listed.

1. Go to the Azure Portal and view the `customers-web` container app revisions.

1. Change the `revision mode` of your container app to `multiple` using the Azure Portal or Azure CLI.

    > Tip: If you're using the Azure Portal you'll see a `Choose revision mode` option at the top of the `Revision management` screen. If you're using the Azure CLI you can use the `az containerapp revision set-mode` command.

1. Continue to the next exercise.

### Exercise 2: Split Traffic for an A/B Test

1. Create a new `Azure Container Registry` resource named `ContainerAppsRegistry<your-last-name>` using the Azure Portal. 

1. Go to the `Overview` screen for your container registry in the Azure Portal and select `Update` from the top toolbar. 

1. Enable the `Admin user` capability and save the changes.

1. Open the `Lab 2` folder from this course's GitHub repository in an editor and note that there are `Dockerfile` and `index.html` files in it. We'll use these files to create a new image that can be used to perform an A/B test for users.

1. Open `index.html` and note that it has an header with a value of `B Test!`.

1. Build the `Dockerfile` located in the `Lab 2/Files` folder by using the following command:

    ```bash
    docker build -t ContainerAppsRegistry<your-last-name>/customers-app:2.0 .
    ```

1. Push the new image to your ACR registry by running the following commands:

    ```bash
    az login
    az acr login --name ContainerAppsRegistry<your-last-name>
    docker push ContainerAppsRegistry<your-last-name>.azurecr.io/customers-app:2.0
    ```

1. Create a new revision in the `customers-web` container app using the `Azure Portal`:

    - image: `ContainerAppsRegistry<your-last-name>.azurecr.io/customers-app:2.0`
    - revision-suffix: `b-test`

    > Tip: Delete the existing `customers-app` container image shown on the `Create and deploy new revision` screen in the Azure Portal before creating the new revision. Otherwise two containers will be run for the revision.

1. After the new `b-test` revision is provisioned, select it in the Azure Portal `Revision management` screen and click the `Revision Url` that is displayed to view it in a browser.

1. Go back to the Azure Portal `Revision management` screen and assign `50%` of the traffic to the `a-test` revision and `50%` of the traffic to the `b-test` revision. Save the updates.

1. Go to the `Overview` screen and select the `Application Url`. Refresh the page that displays multiple times and you *should see* that the `A` and `B` versions of the app should randomly display. 

    > Note: If you don't see both pages displayed feel free to move on. It can take some time for the traffic split to start. As long as each of your revision specific URLs are loading things are working as expected.

1. Bonus (if time permits): Go back to the `Revision management` screen and add a label named `green-app` to the `b-test` revision. Select the revision and notice that a new `Label URL` value shows in the `Revision details` screen. This provides a way to have a consistent URL for various scenarios such as `blue-green deployments`. If the label is moved to another revision the same URL will still work.

1. Delete your Azure Container App. You can leave your `Environment`.
