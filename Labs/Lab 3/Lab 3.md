## Lab 3: Deploy with an External Network

In this lab you'll learn how to work with ingress configuration.

> Tip: Use `az containerapp [command] --help`, visit https://learn.microsoft.com/cli/azure/containerapp, or view the `Solution.md` file to get help with Azure CLI commands.

### Exercise 1: Provide a virtual network to an external Azure Container Apps environment
 
1. Perform the steps at the tutorial below to create a new container app with a custom VNET:

   https://learn.microsoft.com/en-us/azure/container-apps/vnet-custom

1. Visit the URL for your container app to ensure it's working.

### Exercise 2: Modify ingress configuration

1. Login to the Azure CLI if you aren't already logged in:

    ```bash
    az login
    ```

1. Perform the following tasks using the Azure CLI.

1. Show details about the current ingress traffic.

1. Disable the ingress and try to hit the container app URL (it shouldn't work now).

1. Enable the ingress again. Ensure that you set the `type` to `external` and the `target-port` to `80`. After the command completes you should be able to hit the URL again.

1. Try to change the traffic for the `latest` revision from 100 to 80. What error do you get and why?

    > Hint: You'll get an error about the container app being configured for a single revision. Traffic can only be
    changed when you have multiple revisions enabled using `az containerapp revision set-mode -n <container-app> -g <resource-group> --mode Multiple`.

1. Change the container app mode to `Multiple` using the command in the previous `hint`.

1. Try to change the `latest` revision's traffic to `80` again. What error do you get?

1. List the current revision name.

1. Add a new revision with a suffix of `2nd-revision` and set the image to `nginx:alpine`.

1. Change the traffic for the previous revision (the one you looked up the name earlier) to `80` and the `2nd-revision` (ensure that you use the full revision name) to `20`.

    > Note: As a review, this technique could be used for a canary deployment or for A/B testing.
        
1. Hit the URL for your container app and ensure it works.

