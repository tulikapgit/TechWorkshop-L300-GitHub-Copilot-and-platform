# Azure Infrastructure Planning Guide

## Overview

This guide helps you plan the Azure infrastructure for the ZavaStorefront web application before creating your GitHub issue. Proper planning ensures that your infrastructure meets all requirements and follows Azure best practices.

## Architecture Overview

The ZavaStorefront solution requires a cloud-native architecture on Azure with the following components:

### Core Infrastructure Components

1. **Azure Container Registry (ACR)**
   - Purpose: Store Docker container images
   - SKU: Basic or Standard (for dev environment)
   - Authentication: Azure RBAC with managed identity (no passwords)

2. **Linux App Service (Web App for Containers)**
   - Purpose: Host the containerized .NET application
   - Platform: Linux with Docker support
   - Deployment: Pull images from ACR using managed identity
   - Configuration: System-assigned managed identity with AcrPull role

3. **Application Insights**
   - Purpose: Monitor application performance and telemetry
   - Integration: Connected to App Service via instrumentation key
   - Features: Request tracking, dependency monitoring, custom events

4. **Microsoft Foundry**
   - Purpose: AI services (GPT-4 and Phi models)
   - Region: westus3 (confirmed model availability)
   - Configuration: Dev-appropriate SKUs

5. **Log Analytics Workspace**
   - Purpose: Centralized logging and diagnostics
   - Integration: Linked to Application Insights and App Service

## Infrastructure Requirements

### Regional Considerations

- **Primary Region**: westus3
- **Rationale**: 
  - Microsoft Foundry model availability (GPT-4 and Phi)
  - All resources in single region for reduced latency
  - Cost optimization

### Resource Organization

- **Resource Group**: Single resource group for all resources
- **Naming Convention**: `rg-zavastore-dev-westus3`
- **Environment**: Development (dev)
- **Deployment Method**: Azure Developer CLI (AZD) with Bicep

### Security Requirements

1. **No Password-Based Authentication**
   - ACR authentication via managed identity only
   - System-assigned managed identity for App Service
   - AcrPull role assignment in Bicep

2. **RBAC Configuration**
   - Managed identity enabled on App Service
   - Role assignment: AcrPull on ACR
   - Principle of least privilege

3. **Network Security**
   - HTTPS enforced for App Service
   - Secure container registry access

### Development Workflow

1. **No Local Docker Required**
   - Use `az acr build` for cloud-based image builds
   - Alternative: GitHub Actions with hosted runners
   - Container build and push happens in Azure

2. **Infrastructure as Code**
   - All resources defined in Bicep templates
   - Modular structure for maintainability
   - Version controlled infrastructure

## Bicep Structure

### Recommended Folder Structure

```
repository-root/
├── infra/
│   ├── main.bicep                 # Root orchestration template
│   ├── main.parameters.json       # Environment-specific parameters
│   ├── modules/
│   │   ├── acr.bicep             # Container Registry
│   │   ├── appService.bicep      # App Service + App Service Plan
│   │   ├── appInsights.bicep     # Application Insights
│   │   ├── logAnalytics.bicep    # Log Analytics Workspace
│   │   ├── foundry.bicep         # Microsoft Foundry
│   │   └── roleAssignment.bicep  # RBAC role assignments
│   └── README.md                  # Infrastructure documentation
├── azure.yaml                     # AZD configuration
└── src/                          # Application source code
```

### Key Bicep Modules

1. **main.bicep** - Orchestrates all modules and defines parameters
2. **acr.bicep** - Provisions Azure Container Registry
3. **appService.bicep** - Provisions App Service Plan and Web App
4. **appInsights.bicep** - Provisions Application Insights and Log Analytics
5. **roleAssignment.bicep** - Configures managed identity and RBAC
6. **foundry.bicep** - Provisions Microsoft Foundry resources

## Parameters to Define

### Resource Names
- Application name: `zavastore`
- Environment: `dev`
- Region: `westus3`
- Resource group prefix: `rg`

### SKU Configurations
- **ACR SKU**: Basic or Standard
- **App Service Plan**: B1 or F1 (Free tier for dev)
- **Microsoft Foundry**: Dev-appropriate tier
- **Application Insights**: Standard

### Application Configuration
- **Container Image**: Placeholder initially, updated during deployment
- **App Settings**: 
  - Application Insights connection string
  - Microsoft Foundry endpoint
  - Environment variables

## Pre-Deployment Checklist

### Azure Subscription Requirements

- [ ] Active Azure subscription with sufficient credits
- [ ] Contributor or Owner permissions on the subscription
- [ ] Quota available for Microsoft Foundry in westus3
- [ ] Quota available for App Service in westus3
- [ ] Azure CLI installed and authenticated locally

### GitHub Repository Requirements

- [ ] Repository forked to your GitHub account
- [ ] Issues feature enabled in repository settings
- [ ] GitHub Copilot access enabled
- [ ] Visual Studio Code with extensions installed:
  - GitHub Copilot
  - Azure Developer CLI
  - Bicep extension

### Azure Developer CLI (AZD) Setup

- [ ] AZD installed (version 1.0.0 or higher)
- [ ] AZD authenticated with Azure (`azd auth login`)
- [ ] Understanding of AZD workflow:
  - `azd init` - Initialize project
  - `azd provision --preview` - Preview resources
  - `azd provision` - Deploy infrastructure
  - `azd deploy` - Deploy application code
  - `azd up` - Provision + Deploy in one command

## Cost Considerations

### Estimated Monthly Costs (Dev Environment)

- Azure Container Registry (Basic): ~$5/month
- App Service Plan (B1): ~$13/month
- Application Insights: Pay-as-you-go (~$2-5/month for dev workload)
- Log Analytics: Pay-as-you-go (~$2-5/month for dev workload)
- Microsoft Foundry: Variable based on usage

**Total Estimated Cost**: $25-35/month for development environment

### Cost Optimization Tips

1. Use Free tier App Service Plan (F1) where possible
2. Stop/deallocate resources when not in use
3. Set up budget alerts in Azure
4. Use Basic SKU for ACR (sufficient for dev)
5. Review and clean up resources after workshop

## GitHub Issue Template

When creating your infrastructure issue, include the following information:

### Summary
Clear description of what infrastructure needs to be provisioned and why.

### Requirements
- List all Azure resources needed
- Specify SKUs and configurations
- Define authentication and security requirements
- Document monitoring and observability needs

### Implementation Guidance
- Bicep structure and modules
- Naming conventions
- Deployment workflow
- CI/CD considerations

### Checklist
- [ ] Confirm regional quotas and availability
- [ ] Define all Bicep modules
- [ ] Create main orchestration template
- [ ] Configure AZD workflow
- [ ] Set up CI/CD pipeline (if applicable)
- [ ] Configure RBAC and security
- [ ] Wire up monitoring
- [ ] Document deployment process
- [ ] Perform smoke tests

## Next Steps

After completing your infrastructure planning:

1. **Create GitHub Issue**: Use Chat with Copilot to create an infrastructure issue
   - Reference this planning guide
   - Include all requirements identified above
   - Follow the template in [Exercise 02 Task 01](02_01.md)

2. **Generate Bicep Scripts**: Use GitHub Copilot in VS Code to generate infrastructure
   - Reference your GitHub issue
   - Leverage Azure MCP for best practices
   - Follow the workflow in [Exercise 02 Task 02](02_02.md)

3. **Validate and Deploy**: Use AZD to provision and deploy
   - Preview with `azd provision --preview`
   - Deploy with `azd up`
   - Verify in Azure Portal

## Additional Resources

- [Azure Container Registry Best Practices](https://learn.microsoft.com/azure/container-registry/container-registry-best-practices)
- [App Service on Linux](https://learn.microsoft.com/azure/app-service/overview-linux)
- [Application Insights Overview](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview)
- [Azure Developer CLI Documentation](https://learn.microsoft.com/azure/developer/azure-developer-cli/)
- [Bicep Documentation](https://learn.microsoft.com/azure/azure-resource-manager/bicep/)
- [Microsoft Foundry Region Support](https://learn.microsoft.com/azure/ai-foundry/reference/region-support)

## Troubleshooting

### Common Planning Issues

**Issue**: Unsure which region to use
- **Solution**: Use westus3 for Microsoft Foundry model availability (GPT-4, Phi)

**Issue**: Uncertain about SKU selection
- **Solution**: Start with Basic/B1 tiers for dev, scale up as needed

**Issue**: Confusion about authentication method
- **Solution**: Always use managed identity with RBAC (no passwords)

**Issue**: Don't know if Docker is needed locally
- **Solution**: No - use `az acr build` or GitHub Actions for cloud builds

### Getting Help

- Use GitHub Copilot Chat to ask questions about infrastructure
- Reference Azure documentation for service-specific details
- Consult with Azure architects for production planning
- Review the complete issue template in [Exercise 02 Task 01](02_01.md)
