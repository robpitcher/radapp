# Radapp
This was supposed to be a [Radius](https://radapp.io) learning exercise. It still is (mostly), but I also wanted to become more familiar with
[Azure Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview?tabs=bicep), [MegaLinter](https://megalinter.io/latest/), and [Github Actions](https://docs.github.com/en/actions) in the process. Two birds, one stone and all that.

In addition to teaching myself the aforementioned items, a secondary goal for this repo is to provide a starting point for others that want to use the same technologies to setup and play around with Radius.

## Prerequisites
- A Github account
- An Azure Subscription

## Installation
1. Fork your own copy of this repo
2. Create a [service principal](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal), make the appropriate RBAC assignments, and make note of the Client ID and Client Secret value.
3. In your Github repo, go to Settings > Secrets and variables > Actions > New repository secret and create a secret named `AZURE_CREDENTIALS`.
4. Enter the value for the secret like shown below:
```json
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>"
}
```
5. Create another secret named `AZURE_SUBSCRIPTION` and enter the subscription ID (e.g., `96cc8277-a2f2-46d7-ae7f-0c24ca0bf5ea`) for its value.
6. Review the environment variables in `.github\workflows\bicep-deploy.yml` (lines 13-17) and adjust as desired.
7. Run the `Azure Bicep Deployment` workflow in Github Actions and verify it creates the resource group and AKS cluster as expected.

## Megalinter Notes
Megalinter *should* just work for you without any additional configuration. It's currently configured to run only on pull requests to main or master branches. Personally, I also run Megalinter locally while working on feature/fix branches because:
1. It's much faster than waiting on the Github-hosted runners to repeatedly pull the Megalinter docker image on each Megalinter run
2. By the time I go to PR the branch into main, I can go grab a snack and be confident that I'll come back to ✅ checks.

My original intent with Megalinter was to install it and run with default settings. I was setting up a brand new repo from scratch, can't be that bad right? Long story short, I made some changes. Below are the changes I made as well as the reasoning (at the time) for doing so.
- Disabled KICS linter: It was between this or checkov. It got to a point where I was trying to suppress several warnings and I didn't want to have do the same thing multiple times for two different, but similar linters.
- `REPOSITORY_CHECKOV_ARGUMENTS: --skip-framework arm`: This was just duplicating errors from the bicep linter, so I disabled it.
- `APPLY_FIXES: none`: I use this as a learning opportunity.

## Source Guides
Useful for troubleshooting or additional info
- [Quickstart: Deploy Bicep files by using GitHub Actions](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deploy-github-actions?tabs=userlevel%2CCLI#generate-deployment-credentials)
- [Quickstart: Deploy an Azure Kubernetes Service (AKS) cluster using Bicep](https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-bicep?tabs=azure-cli)
