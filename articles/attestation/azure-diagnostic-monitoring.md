---
title: Supervision des diagnostics Azure pour Azure Attestation
description: Supervision des diagnostics Azure pour Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: b031eaaeb3431f3beb8274698b0e7322da9e51b5
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111748530"
---
# <a name="set-up-diagnostics-with-a-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Configurer des diagnostics avec un point de terminaison de Module de plateforme sécurisée (TPM) d’Azure Attestation

Cet article vous aide dans la création et la configuration de paramètres de diagnostic pour envoyer les journaux de plateforme et les métriques de plateforme vers différentes destinations. Les [journaux de plateforme](../azure-monitor/essentials/platform-logs-overview.md) dans Azure, y compris le journal des activités Azure et les journaux de ressources, fournissent des informations de diagnostic et d’audit détaillées pour les ressources Azure et la plateforme Azure dont elles dépendent. Les [métriques de plateforme](../azure-monitor/essentials/data-platform-metrics.md) sont collectées par défaut et stockées dans la base de données Métriques Azure Monitor.

Avant de commencer, assurez-vous d’avoir [configuré Azure Attestation avec Azure PowerShell](quickstart-powershell.md).

Le service de point de terminaison TPM est activé dans les paramètres de diagnostic et peut être utilisé pour superviser l’activité. Configurez la [Supervision Azure](../azure-monitor/overview.md) pour le point de terminaison du service TPM en utilisant le code suivant.

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription "<Subscription id>"

 $attestationProviderName="<Name of the attestation provider>"

 $attestationResourceGroup="<Name of the resource Group>"

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name "<Storage Account Name>" -SkuName Standard_LRS -Location "<Location>"

 Set-AzDiagnosticSetting -ResourceId $attestationProvider.Id -StorageAccountId $storageAccount.Id -Enabled $true 

```

Les journaux d’activité se trouvent dans la section **Conteneurs** du compte de stockage. Pour plus d’informations, consultez [Collecter et analyser des journaux de ressources à partir d’une ressource Azure](../azure-monitor/essentials/tutorial-resource-logs.md).
