# Azure Infrastructure Quick Reference

This is a condensed quick reference for planning Azure infrastructure for GitHub Issue #1 (ZavaStorefront deployment).

## What You're Building

A containerized .NET web application on Azure with:
- Container Registry (ACR) for Docker images
- Linux App Service to run containers
- Application Insights for monitoring
- Microsoft Foundry for AI (GPT-4, Phi)

## Quick Requirements Checklist

### ✅ Azure Resources Needed

- [ ] Azure Container Registry (Basic SKU)
- [ ] Linux App Service (B1 or F1 tier)
- [ ] App Service Plan (Linux)
- [ ] Application Insights
- [ ] Log Analytics Workspace
- [ ] Microsoft Foundry (westus3)
- [ ] Resource Group (single group for all resources)

### ✅ Configuration Requirements

- [ ] Region: **westus3** (for Foundry model availability)
- [ ] Environment: **dev**
- [ ] Naming: `rg-zavastore-dev-westus3`
- [ ] Authentication: Managed Identity + RBAC (no passwords)
- [ ] Deployment: Azure Developer CLI (AZD) + Bicep

### ✅ Security Setup

- [ ] System-assigned managed identity on App Service
- [ ] AcrPull role assigned to managed identity
- [ ] No password-based ACR authentication
- [ ] HTTPS enforced on App Service

### ✅ Development Workflow

- [ ] No local Docker required
- [ ] Use `az acr build` for container builds
- [ ] All infrastructure defined in Bicep
- [ ] Deploy with `azd up`

## Folder Structure to Create

```
/infra/
  main.bicep
  main.parameters.json
  /modules/
    acr.bicep
    appService.bicep
    appInsights.bicep
    logAnalytics.bicep
    foundry.bicep
    roleAssignment.bicep
  README.md
/azure.yaml
```

## Issue #1 Key Points

When creating your GitHub issue, include:

1. **Summary**: Provision Azure infrastructure for ZavaStorefront in westus3
2. **Resources**: ACR, App Service, App Insights, Foundry, Log Analytics
3. **Requirements**: 
   - Single resource group
   - Managed identity authentication
   - No local Docker
   - Bicep + AZD deployment
4. **Checklist**: All Bicep modules, role assignments, monitoring, deployment docs

## Quick Commands

```bash
# Initialize AZD
azd init

# Preview deployment
azd provision --preview

# Deploy everything
azd up

# Build container in cloud
az acr build --registry <acr-name> --image zavastore:latest .
```

## Estimated Cost (Dev)

~$25-35/month total:
- ACR Basic: ~$5
- App Service B1: ~$13
- Application Insights: ~$2-5
- Log Analytics: ~$2-5
- Foundry: Variable (usage-based)

## Common Copilot Prompts

### Creating the Issue
```
/create-issue Provision Azure infrastructure for the ZavaStorefront web
application. It will need a Linux App Service, and I want to use Docker to
deploy to the App Service, so I'll need a Container Registry as well. But I
don't want to install docker on my local machine. The AppService should use
Azure RBAC to pull images not passwords. I want to monitor it with
Application Insights. I will be using GPT-4 and Phi so I will need Microsoft
Foundry, I want to use westus3 because they are available there. Everything
should be deployed together in one resource group in the same region. This
is a dev environment and I want to use AZD with Bicep to define and deploy
everything.
```

### Generating Infrastructure
```
plan azure infrastructure for the github issue 1
```

### Fixing Issues
```
Can you explain and fix the latest provision error?
```

## Pre-Flight Checklist

Before starting:
- [ ] Azure subscription active
- [ ] Azure CLI authenticated
- [ ] AZD installed and authenticated
- [ ] GitHub Copilot enabled
- [ ] VS Code extensions installed (Copilot, Bicep, Azure)
- [ ] Repository forked and cloned
- [ ] Issues enabled in repository

## Next Steps

1. ✅ Review this quick reference
2. ✅ Create GitHub Issue #1 using Copilot Chat
3. ✅ Generate Bicep scripts in VS Code with Copilot
4. ✅ Run `azd provision --preview` to validate
5. ✅ Deploy with `azd up`
6. ✅ Verify resources in Azure Portal

## Need More Details?

See the complete [Azure Infrastructure Planning Guide](AZURE_INFRASTRUCTURE_PLANNING.md) for:
- Detailed architecture overview
- Security best practices
- Troubleshooting guide
- Cost optimization tips
- Additional resources and links
