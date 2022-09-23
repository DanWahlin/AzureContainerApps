## Lab 2 Solution

Here are the commands used in this Lab.

### Exercise 1: Create an Azure Container App

#### Create a Container App

```bash
az containerapp create -n customers-web -g my-container-apps \ 
--environment my-environment \
--image nginx:1.17.8-alpine \
--ingress external --target-port 80 \
--container-name customers-web \
--min-replicas 1 --max-replicas 5 \
--query properties.configuration.ingress.fqdn
```

#### List Revisions

```bash
az containerapp revision list -n customers-web -g my-container-apps -o table
```

#### Show a Revision

```bash
az containerapp revision show --revision customers-web--<suffix-value> -n customers-web -g my-container-apps -o yaml
```

#### Update a Revision Image (creates a new revision)

```bash
az containerapp update -n customers-web -g my-container-apps --image nginx:1.23.1-alpine --revision-suffix a-test
```

#### Change Revision Mode to Multiple

```bash
az containerapp revision set-mode --mode multiple -n customers-web -g my-container-apps
```









