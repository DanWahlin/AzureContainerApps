## Lab 3 Solution

Here are the commands used in this Lab.

### Exercise 1: Provide a virtual network to an external Azure Container Apps environment
 
Steps can be found at https://learn.microsoft.com/en-us/azure/container-apps/vnet-custom

### Exercise 2: Modify ingress configuration

#### Show Ingress Traffic Details

```bash
az containerapp ingress traffic show -n <container-app> -g <resource-group>
```

### Disable Ingress

```bash
az containerapp ingress disable -n <container-app> -g <resource-group>
```

### Enable Ingress

```bash
az containerapp ingress enable -n <container-app> -g <resource-group> --type external --target-port 80
```

#### Set Ingress Traffic for Latest Revision

```bash
az containerapp ingress traffic set -n <container-app> -g <resource-group> --revision-weight latest=80
```

#### Change the Revision Mode to Multiple

```bash
az containerapp revision set-mode -n <container-app> -g <resource-group> --mode Multiple
```

#### Update a Revision

```bash
az containerapp update -n <container-app> -g <resource-group> --image nginx:alpine --revision-suffix 2nd-revision
```

#### Set Ingress Traffic for Revisions

```bash
az containerapp ingress traffic set -n <container-app> -g <resource-group> --revision-weight <previous-reversion-name>=80 <container-app>--2nd-revision=20
```

