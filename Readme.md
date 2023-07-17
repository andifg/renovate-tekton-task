# Renovate Tekton Task

This project aims to create a [Tekton](https://tekton.dev/) Task that triggers the [renovate](https://docs.renovatebot.com/) dependency
update bot with proper default settings. The task can then be used by mutliple tenants to run renovate with their projects by only using a 
Tekton Task run and a few params by utilizing tekton resolvers.

## Limitations

- The Task uses PAT tokens to authenticate with the Git provider. So its not possible to use e.g. a Github App.


## How to integrate renovate with your project

1. Copy the [example task run](docs/example-taskRun-github.yaml) to your project.
2. Configure params according to your need. All possible params can be seen in the  renovate/<version>/task.yaml within each version of the renovate bot.
3. Make sure to have the token used to authenticate against your git provider available inside a secret in the target namespace (secret name is input to renovate-token-secret-name param).

This oc command can be used for secret creation:
```bash
oc create secret generic renovate-api-token-github --from-literal token=$API_TOKEN
```
