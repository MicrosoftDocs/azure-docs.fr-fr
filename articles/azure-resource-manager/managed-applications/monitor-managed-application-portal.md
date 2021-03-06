---
title: Utiliser le Portail Azure pour effectuer le monitorage d’une application managée
description: Présente comment utiliser le portail Azure pour surveiller la disponibilité et les alertes pour une application managée.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: a02062edd1a940bcc6588ab53457e0af91fedd9a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100578279"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Surveiller une instance déployée d’une application managée

Une fois que vous avez déployé une application managée sur votre abonnement Azure, vous souhaiterez probablement vérifier l’état de l’application. Cet article détaille les options de vérification de l’état dans le portail Azure. Vous pouvez surveiller la disponibilité des ressources dans votre application managée. Vous pouvez également définir et afficher des alertes.

## <a name="view-resource-health"></a>Afficher l’intégrité des ressources

1. Sélectionnez votre instance de l’application managée.

   ![Sélectionner une application managée](./media/monitor-managed-application-portal/select-managed-application.png)

1. Sélectionnez **Intégrité des ressources**.

   ![Sélectionner l’intégrité des ressources](./media/monitor-managed-application-portal/select-resource-health.png)

1. Affichez la disponibilité des ressources dans votre application managée.

   ![Afficher l’intégrité des ressources](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Afficher les alertes

1. Sélectionnez **Alertes**.

   ![Sélectionner les alertes](./media/monitor-managed-application-portal/select-alerts.png)

1. Si vous avez configuré les règles d’alerte, des informations sur les alertes qui ont été générées s’affichent.

   ![Afficher les alertes](./media/monitor-managed-application-portal/view-alerts.png)

1. Pour ajouter des règles d’alerte, sélectionnez **+ nouvelle règle d’alerte**.

   ![Créer une alerte](./media/monitor-managed-application-portal/create-new-alert.png)

Vous pouvez créer des alertes pour votre instance d’application managée ou les ressources dans l’application managée. Pour plus d’informations sur la création d’alertes, consultez [Vue d’ensemble des alertes dans Microsoft Azure](../../azure-monitor/alerts/alerts-overview.md).

## <a name="next-steps"></a>Étapes suivantes

* Pour obtenir des exemples d’applications managées, consultez [Exemples de projets pour des applications managées Azure](sample-projects.md).
* Pour déployer une instance de l’application managée, consultez [Deploy service catalog app through Azure portal](deploy-service-catalog-quickstart.md) (Déployer une application de catalogue de services via le Portail Azure).