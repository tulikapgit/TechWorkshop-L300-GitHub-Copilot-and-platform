# Azure Infrastructure Architecture Diagram

## ZavaStorefront Infrastructure Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        Azure Subscription                                │
│                                                                          │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │  Resource Group: rg-zavastore-dev-westus3 (westus3)               │ │
│  │                                                                    │ │
│  │  ┌──────────────────────────────────────────────────────────┐    │ │
│  │  │  Application Layer                                        │    │ │
│  │  │                                                            │    │ │
│  │  │  ┌─────────────────────────────────────────┐            │    │ │
│  │  │  │  Linux App Service                       │            │    │ │
│  │  │  │  (Web App for Containers)                │            │    │ │
│  │  │  │                                           │            │    │ │
│  │  │  │  ┌────────────────────────────────────┐ │            │    │ │
│  │  │  │  │  System-Assigned                   │ │            │    │ │
│  │  │  │  │  Managed Identity                  │ │            │    │ │
│  │  │  │  │  (AcrPull Role)                    │ │            │    │ │
│  │  │  │  └────────────────────────────────────┘ │            │    │ │
│  │  │  │                                           │            │    │ │
│  │  │  │  • Docker container runtime               │            │    │ │
│  │  │  │  • HTTPS enabled                          │            │    │ │
│  │  │  │  • Pulls images via RBAC                 │            │    │ │
│  │  │  └─────────────────────────────────────────┘            │    │ │
│  │  │            │                                              │    │ │
│  │  │            │ pulls images                                │    │ │
│  │  │            ▼                                              │    │ │
│  │  │  ┌─────────────────────────────────────────┐            │    │ │
│  │  │  │  Azure Container Registry (ACR)          │            │    │ │
│  │  │  │                                           │            │    │ │
│  │  │  │  • Basic/Standard SKU                    │            │    │ │
│  │  │  │  • Azure RBAC authentication             │            │    │ │
│  │  │  │  • Container image storage               │            │    │ │
│  │  │  │  • Cloud-based builds (az acr build)    │            │    │ │
│  │  │  └─────────────────────────────────────────┘            │    │ │
│  │  └──────────────────────────────────────────────────────────┘    │ │
│  │                                                                    │ │
│  │  ┌──────────────────────────────────────────────────────────┐    │ │
│  │  │  Monitoring & Observability                              │    │ │
│  │  │                                                            │    │ │
│  │  │  ┌─────────────────────────────────────────┐            │    │ │
│  │  │  │  Application Insights                    │            │    │ │
│  │  │  │                                           │            │    │ │
│  │  │  │  • Request tracking                       │            │    │ │
│  │  │  │  • Dependency monitoring                 │            │    │ │
│  │  │  │  • Performance metrics                   │            │    │ │
│  │  │  │  • Custom telemetry                      │            │    │ │
│  │  │  └─────────────────────────────────────────┘            │    │ │
│  │  │            │                                              │    │ │
│  │  │            │ connected to                                │    │ │
│  │  │            ▼                                              │    │ │
│  │  │  ┌─────────────────────────────────────────┐            │    │ │
│  │  │  │  Log Analytics Workspace                 │            │    │ │
│  │  │  │                                           │            │    │ │
│  │  │  │  • Centralized logging                   │            │    │ │
│  │  │  │  • Query and analysis                    │            │    │ │
│  │  │  │  • Diagnostic data storage               │            │    │ │
│  │  │  └─────────────────────────────────────────┘            │    │ │
│  │  └──────────────────────────────────────────────────────────┘    │ │
│  │                                                                    │ │
│  │  ┌──────────────────────────────────────────────────────────┐    │ │
│  │  │  AI Services                                              │    │ │
│  │  │                                                            │    │ │
│  │  │  ┌─────────────────────────────────────────┐            │    │ │
│  │  │  │  Microsoft Foundry                        │            │    │ │
│  │  │  │                                           │            │    │ │
│  │  │  │  • GPT-4 model access                    │            │    │ │
│  │  │  │  • Phi model access                       │            │    │ │
│  │  │  │  • westus3 region for availability       │            │    │ │
│  │  │  │  • Dev-appropriate SKU                   │            │    │ │
│  │  │  └─────────────────────────────────────────┘            │    │ │
│  │  └──────────────────────────────────────────────────────────┘    │ │
│  └───────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────┘
```

## Infrastructure Components

### 1. **Application Hosting**
   - **Linux App Service (Web App for Containers)**
     - Runs the ZavaStorefront .NET application in a Docker container
     - System-assigned managed identity for secure authentication
     - Configured to pull container images from ACR using RBAC

### 2. **Container Registry**
   - **Azure Container Registry (ACR)**
     - Stores Docker images for the application
     - Authenticated via Azure RBAC (no passwords)
     - Supports cloud-based image builds without local Docker

### 3. **Monitoring**
   - **Application Insights**
     - Monitors application performance and behavior
     - Tracks requests, dependencies, and custom events
     - Connected to Log Analytics for advanced querying
   
   - **Log Analytics Workspace**
     - Centralized logging and diagnostics
     - Enables complex queries across logs
     - Provides data for dashboards and alerts

### 4. **AI Services**
   - **Microsoft Foundry**
     - Provides access to GPT-4 and Phi AI models
     - Deployed in westus3 for model availability
     - Enables AI-powered features in the application

## Data Flow

### Deployment Flow
```
Developer → GitHub → Copilot generates Bicep → AZD deploys → Azure Resources
                                                              ↓
Source Code → az acr build → Container Image → ACR → App Service pulls
```

### Application Flow
```
User Request → App Service → Application Logic → Response
                    ↓
              App Insights (telemetry)
                    ↓
              Log Analytics (storage)
```

### Authentication Flow
```
App Service → Managed Identity → Azure AD → ACR (AcrPull role) → Pull Image
```

## Security Architecture

### Identity & Access Management
- **System-Assigned Managed Identity**: Automatically managed by Azure
- **RBAC Roles**: AcrPull role assigned to App Service identity for ACR access
- **No Credentials**: No passwords or connection strings for ACR authentication

### Network Security
- **HTTPS Only**: App Service enforces HTTPS for all requests
- **Secure Registry Access**: ACR accessible only via authenticated identities
- **Private Communication**: All Azure services communicate within Azure backbone

## Deployment Architecture

### Infrastructure as Code (IaC)
```
repository-root/
├── infra/
│   ├── main.bicep              (orchestrates all modules)
│   ├── main.parameters.json    (environment parameters)
│   └── modules/
│       ├── acr.bicep           (container registry)
│       ├── appService.bicep    (web app + plan)
│       ├── appInsights.bicep   (monitoring)
│       ├── logAnalytics.bicep  (logging)
│       ├── foundry.bicep       (AI services)
│       └── roleAssignment.bicep (RBAC)
└── azure.yaml                  (AZD configuration)
```

### Deployment Tool
- **Azure Developer CLI (AZD)**
  - `azd init` - Initialize project
  - `azd provision` - Deploy infrastructure
  - `azd deploy` - Deploy application code
  - `azd up` - Provision + Deploy combined

## High Availability & Scaling

### Current Configuration (Dev)
- **App Service**: Single instance (B1/F1 tier)
- **ACR**: Basic tier with geo-replication disabled
- **Application Insights**: Standard tier
- **No auto-scaling**: Manual scaling only

### Production Considerations (Future)
- Upgrade to Standard/Premium App Service tiers
- Enable auto-scaling based on metrics
- Implement geo-redundancy for ACR
- Add Azure Front Door or Application Gateway
- Implement backup and disaster recovery

## Cost Breakdown (Dev Environment)

| Resource | SKU | Est. Cost/Month |
|----------|-----|-----------------|
| Azure Container Registry | Basic | ~$5 |
| App Service Plan | B1 | ~$13 |
| Application Insights | Pay-as-you-go | ~$2-5 |
| Log Analytics | Pay-as-you-go | ~$2-5 |
| Microsoft Foundry | Usage-based | Variable |
| **Total** | | **~$25-35** |

## References

- [Detailed Planning Guide](AZURE_INFRASTRUCTURE_PLANNING.md)
- [Quick Reference](QUICK_REFERENCE.md)
- [Exercise Documentation](README.md)
