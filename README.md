# Confidential Games

A collection of scripts to deploy game servers using Azure Confidential Computing and Azure Container Instances (ACI).

## Overview

This repository contains PowerShell scripts that demonstrate how to deploy classic games in secure, confidential computing environments using Azure Container Instances with confidential SKUs.

## Current Games

### Doom (BuildRandomDoomACI.ps1)

Deploy the classic Doom game in a confidential Azure Container Instance with VNC access.

#### Prerequisites

- Azure PowerShell module (latest version required for ACI confidential computing support)
- Azure subscription with access to confidential computing resources
- Valid Azure account with appropriate permissions

#### Usage

```powershell
./BuildRandomDoomACI.ps1 -subsID <YOUR_SUBSCRIPTION_ID> -basename <YOUR_BASENAME>
```

Then connect to the IP address or DNS name (shown at the end of the script) of the container on port 8080 using your web browser e.g. http://1.2.3.4:8080

**Parameters:**
- `subsID` (mandatory): Your Azure subscription ID
- `basename` (mandatory): A prefix for all created resources (5 random lowercase letters will be appended)

#### What it creates

- **Resource Group**: Named after your basename
- **Container Instance**: Runs the `b0nam/docker-doom` Docker image
- **Public IP**: With DNS label for external access
- **VNC Server**: Accessible on port 6901 (UDP)

#### Connection Details

- **VNC Port**: 6901
- **VNC Password**: `PASSWORD`
- **IP Address**: Displayed in the script output after deployment
- **FQDN**: `{basename}dns.{region}.azurecontainer.io`

#### Example

```powershell
./BuildRandomDoomACI.ps1 -subsID "12345678-1234-1234-1234-123456789012" -basename "mydoom"
```

This creates resources with names like:
- Resource Group: `mydoomabcde`
- Container: `mydoomabcdeaci`
- DNS: `mydoomabcdedns.northeurope.azurecontainer.io`

#### Configuration

The script is configured for:
- **Region**: North Europe (ensure your subscription supports confidential computing in this region)
- **SKU**: Confidential
- **OS**: Linux
- **Image**: `b0nam/docker-doom` from Docker Hub

#### Security Features

- Utilizes Azure Confidential Computing for enhanced security
- Resource tagging with owner information
- Confidential SKU ensures workload isolation

#### Cleanup

The script includes commented cleanup commands at the bottom. Uncomment these lines to automatically remove resources after use:

```powershell
Remove-AzContainerGroup -ResourceGroupName $resgrp -Name $containerGroupName
Get-azresourceGroup -name $resgrp | Remove-AzResourceGroup
```

## Important Notes

⚠️ **Warning**: This script uses community-contributed Docker images from Docker Hub. Please review and validate images before using in production environments.

🔒 **Security**: This is designed for demonstration and testing purposes. Review security implications before deploying in production.

💰 **Cost**: Remember to clean up resources to avoid unnecessary charges.

## Contributing

Feel free to contribute additional game deployments or improvements to existing scripts.

## License

Use at your own risk. No warranties implied. Test in non-production environments first.

---

Based on the Azure Confidential Computing samples and Microsoft Learn documentation.