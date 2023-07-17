# Renovate Tekton Task

This project provides a [Tekton](https://tekton.dev/) Task that triggers the [renovate](https://docs.renovatebot.com/) dependency update bot with proper default settings. The task can then be used by mutliple tenants to run renovate with their projects by only using a Tekton Task run and a few params by utilizing tekton git resolvers.

Current released version of the renovate tekton task is 0.1
## Limitations

- Currently only tested for GitHub integration via webhook.
- The Task uses PAT tokens to authenticate with the Git provider. So its not possible to use e.g. a Github App.
- Currently its not possible to just scrape all repositories accessable with the provided token.

## How to integrate renovate with your project

1. Copy the [example task run](docs/example-taskRun-github%20copy.yaml) to your project.
2. Configure params according to your need. All possible params can be seen in the  renovate/<version>/task.yaml within each version of the renovate bot. The explanations for the renovate config parameters can be
found [here](https://docs.renovatebot.com/configuration-options/)
3. Make sure to have the token used to authenticate against your git provider available inside a secret in the target namespace (secret name is input to renovate-token-secret-name param).

This oc command can be used for secret creation (Use this only for testing, proper gitops setup is highly recommended for 
production setup):
```bash
oc create secret generic renovate-api-token-github --from-literal token=$API_TOKEN
```
4. (Optional) Create pvc for renovate cache. The cache is mounted on the renovate working directory and speeds up the process. 
The template in the docs directory can be used to create a pvc:
```bash
oc apply -f docs/pvc-template.yaml -n $TARGET_NAMESPACE
```

5. Setup CronJob or TaskRun 

Its possible to trigger renovate manually via a single TaskRun: 

```bash
oc apply -f docs/example-taskRun-github.yaml
```

Setup of cron execution:

To automatically execute the renovate bot the tekton trigger stack can be used. The [docs/cron-setup directory](docs/cron-setup/) contains a  CronJob, an EventListener and a TriggerTemplate. These resources enable the triggering of the renovate bot according to the schedule referenced in the [TriggerTemplate resource](docs/cron-setup/triggerTemplate.yaml).
To deploy the stack update the kustomize.yaml and deploy it via:

- Copy your taskrun config to the triggertemplate section of the [TriggerTemplate](docs/cron-setup/triggerTemplate.yaml)
- update target namespace in [kustomize](docs/cron-setup/kustomize.yaml) first 
- If you are not using the default service account pipeline from openshift pipeline, adapt the service account in the [eventListener](docs/cron-setup/eventListener.yaml)
```bash
oc apply -f docs/cron-setup/
```
