---
title: Mettre à jour une application sur un cluster dans sfctl
description: Exemple de script CLI Service Fabric - Mettre à jour une application avec une nouvelle version. Cet exemple met également à niveau une application déployée avec les nouveaux bits.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 12/06/2017
ms.author: atsenthi
ms.custom: ''
ms.openlocfilehash: 34f2ae6f3a2ff3adc35794d6e7dfd682640c646b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "75614721"
---
# <a name="update-an-application-using-the-service-fabric-cli"></a>Mettre à jour une application à l’aide de l’interface CLI Service Fabric

Cet exemple de script télécharge une nouvelle version d’une application existante, puis met à niveau une application déployée avec les nouveaux bits.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Exemple de script

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez la [documentation relative à Service Fabric CLI](../service-fabric-cli.md).

Vous trouverez des exemples Service Fabric CLI supplémentaires pour Azure Service Fabric dans la rubrique [Exemples Service Fabric CLI](../samples-cli.md).
