---
title: Azure PowerShell script sample - Deploy a managed application
description: Provides Azure PowerShell sample script sample that deploys a managed application definition to the subscription.
author: davidsmatlak

ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: davidsmatlak
---

# Deploy a managed application for a service catalog with PowerShell

This script deploys a managed application definition from the service catalog.


[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## Sample script

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-application/create-application.ps1 "Create application")]


## Script explanation

This script uses the following command to deploy the managed application. Each command in the table links to command-specific documentation.

| Command | Notes |
|---|---|
| [New-AzManagedApplication](/powershell/module/az.resources/new-azmanagedapplication) | Create a managed application. Provide the definition ID and parameters for the template. |


## Next steps

* For an introduction to managed applications, see [Azure Managed Application overview](../overview.md).
* For more information on PowerShell, see [Azure PowerShell documentation](/powershell/azure/get-started-azureps).
