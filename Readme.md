# Renovate Tekton Task

This project aims to create a [Tekton](https://tekton.dev/) Task that triggers the [renovate](https://docs.renovatebot.com/) dependency
update bot with proper default settings. The task can then be used by mutliple tennants to run renovate with their projects by only using a 
Tekton Task run and a few params.

## Limitations

- The Task uses PAT tokens to authenticate with the Git provider. So its not possible to use e.g. a Github App.


## How to integrate with your project

-> soon to be ready 


