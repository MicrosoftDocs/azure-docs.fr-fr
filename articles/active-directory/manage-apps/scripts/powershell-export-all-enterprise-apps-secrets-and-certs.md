---
title: Exemple PowerShell - Exportez les secrets et certificats relatifs aux applications d’entreprise vers le locataire Azure Active Directory.
description: Exemple PowerShell qui exporte tous les secrets et certificats des applications d’entreprise spécifiées vers votre locataire Azure Active Directory.
services: active-directory
author: mtillman
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 03/09/2021
ms.author: mtillman
ms.reviewer: mifarca
ms.openlocfilehash: 08fc5558cfe7b3459189f168e465ee2fa88d992a
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/14/2021
ms.locfileid: "112076795"
---
# <a name="export-secrets-and-certificates-for-enterprise-apps"></a>Exporter les secrets et certificats des applications d’entreprise
Cet exemple de script PowerShell exporte tous les secrets, certificats et propriétaires des applications d’entreprise spécifiées, de votre annuaire vers un fichier CSV.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

Cet exemple requiert le [module AzureAD v2 PowerShell pour Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) ou la [préversion du Module AzureAD v2 PowerShell pour Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview).

## <a name="sample-script"></a>Exemple de script

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-all-enterprise-apps-secrets-and-certs.ps1 "Exports all secrets and certificates for the specified enterprise apps in your directory.")]

## <a name="script-explanation"></a>Explication du script

La commande « Add-Member » est chargée de créer les colonnes dans le fichier CSV.
Vous pouvez modifier la variable « $Path » directement dans PowerShell, avec un chemin de fichier CSV, si vous préférez que l’exportation ne soit pas interactive.

| Commande | Notes |
|---|---|
| [Get-AzureADServicePrincipal](/powershell/module/azuread/Get-azureADServicePrincipal?view=azureadps-2.0&preserve-view=true) | Récupère une application d’entreprise à partir de votre annuaire. |
| [Get-AzureADServicePrincipalOwner](/powershell/module/azuread/Get-AzureADServicePrincipalOwner?view=azureadps-2.0&preserve-view=true) | Récupère les propriétaires d’une application d’entreprise à partir de votre annuaire. |


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le Module Azure AD PowerShell, consultez [Présentation du Module Azure AD PowerShell](/powershell/azure/active-directory/overview).

Pour d’autres exemples PowerShell de gestion d’applications, consultez [Exemples PowerShell pour la gestion d’applications Azure AD](../app-management-powershell-samples.md).
