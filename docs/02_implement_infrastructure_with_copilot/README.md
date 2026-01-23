# Exercise 02: Implement Infrastructure with Copilot - Documentation

This directory contains all documentation and resources for Exercise 02, which guides you through using GitHub Copilot to plan, create, and deploy Azure infrastructure for the ZavaStorefront application.

## Documentation Structure

### Main Exercise Documentation

- **[02_implement_infrastructure_with_copilot.md](02_implement_infrastructure_with_copilot.md)** - Main exercise overview, objectives, and duration

### Task Guides

- **[02_01.md](02_01.md)** - Task 01: Create an infrastructure issue in GitHub
  - Learn how to use Chat with Copilot to create infrastructure issues
  - Set up your GitHub repository for issue tracking
  - Generate a comprehensive infrastructure issue

- **[02_02.md](02_02.md)** - Task 02: Create Bicep scripts in Visual Studio Code
  - Use GitHub Copilot to generate Bicep infrastructure scripts
  - Work with Azure MCP Server for best practices
  - Deploy infrastructure using Azure Developer CLI (AZD)

### Planning Resources

- **[QUICK_REFERENCE.md](QUICK_REFERENCE.md)** - Quick reference guide
  - Condensed checklist of all requirements
  - Essential commands and prompts
  - Pre-flight checklist
  - Estimated costs
  - Common Copilot prompts

- **[AZURE_INFRASTRUCTURE_PLANNING.md](AZURE_INFRASTRUCTURE_PLANNING.md)** - Comprehensive planning guide
  - Detailed architecture overview
  - Infrastructure components and requirements
  - Bicep structure and module design
  - Security and RBAC configuration
  - Cost considerations and optimization
  - Pre-deployment checklist
  - Troubleshooting guidance

## Learning Path

Follow this recommended sequence:

1. **Start Here**: Read the main [exercise overview](02_implement_infrastructure_with_copilot.md)
2. **Plan**: Review the [Quick Reference Guide](QUICK_REFERENCE.md) for an overview
3. **Deep Dive**: Study the [Azure Infrastructure Planning Guide](AZURE_INFRASTRUCTURE_PLANNING.md) for detailed requirements
4. **Execute Task 01**: Follow [02_01.md](02_01.md) to create your GitHub issue
5. **Execute Task 02**: Follow [02_02.md](02_02.md) to generate and deploy infrastructure

## Quick Start

If you're already familiar with Azure and want to get started quickly:

1. Review the [Quick Reference Guide](QUICK_REFERENCE.md)
2. Jump to [Task 01](02_01.md) to create your GitHub issue
3. Proceed to [Task 02](02_02.md) to generate and deploy

## What You'll Build

By the end of this exercise, you'll have:

- ✅ A GitHub issue describing Azure infrastructure requirements
- ✅ Bicep templates for all Azure resources
- ✅ Azure Developer CLI (AZD) configuration
- ✅ Deployed infrastructure in Azure including:
  - Azure Container Registry
  - Linux App Service
  - Application Insights
  - Microsoft Foundry
  - Log Analytics Workspace
  - Proper RBAC and security configuration

## Prerequisites

Before starting this exercise, ensure you have:

- Completed Exercise 01 (Development Environment Setup)
- Azure subscription with appropriate permissions
- Azure CLI installed and authenticated
- Azure Developer CLI (AZD) installed
- Visual Studio Code with extensions:
  - GitHub Copilot
  - Bicep
  - Azure Developer CLI
- GitHub repository forked and cloned
- Issues enabled in your GitHub repository

## Key Technologies

This exercise uses:

- **GitHub Copilot** - AI-powered code generation and assistance
- **Azure MCP Server** - Best practices and guidance for Azure
- **Bicep** - Infrastructure as Code for Azure
- **Azure Developer CLI (AZD)** - Deployment orchestration
- **Azure Container Registry** - Container image storage
- **Azure App Service** - Application hosting
- **Application Insights** - Application monitoring
- **Microsoft Foundry** - AI services (GPT-4, Phi)

## Common Issues and Solutions

### Issue: Can't find the planning guides
**Solution**: All planning documents are in this directory:
- `QUICK_REFERENCE.md`
- `AZURE_INFRASTRUCTURE_PLANNING.md`

### Issue: GitHub issue doesn't have enough detail
**Solution**: Reference the complete issue template in [02_01.md](02_01.md#expand-this-section-for-a-complete-issue-text)

### Issue: AZD structure validation fails
**Solution**: See the "AZD Sanity Check" section in [02_02.md](02_02.md#azd-sanity-check)

### Issue: Deployment errors during `azd up`
**Solution**: Follow the troubleshooting workflow in [02_02.md](02_02.md#expand-this-section-to-view-troubleshooting-techniques-if-you-have-issues)

## Additional Resources

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Azure Developer CLI Documentation](https://learn.microsoft.com/azure/developer/azure-developer-cli/)
- [Bicep Documentation](https://learn.microsoft.com/azure/azure-resource-manager/bicep/)
- [Azure Container Registry Best Practices](https://learn.microsoft.com/azure/container-registry/container-registry-best-practices)
- [Microsoft Foundry Region Support](https://learn.microsoft.com/azure/ai-foundry/reference/region-support)

## Feedback and Contributions

This is a living document. If you find issues or have suggestions for improvement, please create an issue or submit a pull request.

---

**Ready to begin?** Start with the [Quick Reference Guide](QUICK_REFERENCE.md) or dive into [Task 01](02_01.md)!
