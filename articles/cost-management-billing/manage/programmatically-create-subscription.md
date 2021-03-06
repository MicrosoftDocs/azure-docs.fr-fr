---
title: Créer des abonnements Azure programmatiquement
description: Cet article explique les différentes méthodes permettant de créer des abonnements Azure programmatiquement.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 03/11/2021
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: ce08ebf473b11eecae327c7de050c791f5bc1b1a
ms.sourcegitcommit: 2654d8d7490720a05e5304bc9a7c2b41eb4ae007
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "107379007"
---
# <a name="create-azure-subscriptions-programmatically"></a>Créer des abonnements Azure programmatiquement

Cet article explique les différentes méthodes permettant de créer des abonnements Azure programmatiquement.

À l’aide des différentes API REST, vous pouvez créer un abonnement pour les types de contrats Azure suivants :

- Contrat Entreprise (EA)
- Contrat client Microsoft (MCA)
- Contrat Partenaire Microsoft (MPA)

Vous ne pouvez pas créer des abonnements supplémentaires pour d’autres types de contrats par programmation à l’aide des API REST.

Les spécifications et les informations permettant de créer des abonnements diffèrent selon les contrats et les versions d’API. Consultez les articles suivants qui s’appliquent à votre situation :

API les plus récentes :

- [Créer des abonnements EA](programmatically-create-subscription-enterprise-agreement.md)
- [Créer des abonnements MCA](programmatically-create-subscription-microsoft-customer-agreement.md)
- [Créer des abonnements MPA](programmatically-create-subscription-microsoft-partner-agreement.md)

Ces articles montrent également comment créer des abonnements avec un modèle ARM (Azure Resource Manager). Un modèle ARM permet d’automatiser le processus de création d’abonnement.

Si vous utilisez encore des [API en préversion](programmatically-create-subscription-preview.md), vous pouvez continuer à les utiliser pour créer des abonnements. 

## <a name="next-steps"></a>Étapes suivantes

* Une fois que vous avez créé un abonnement, vous pouvez accorder cette capacité à d’autres utilisateurs et principaux de service. Pour plus d’informations, consultez [Accorder l’accès pour créer des abonnements Azure Enterprise (préversion)](grant-access-to-create-subscription.md).
* Pour plus d’informations sur la gestion d’un grand nombre d’abonnements à l’aide de groupes d’administration, consultez [Organiser vos ressources avec des groupes d’administration Azure](../../governance/management-groups/overview.md).
